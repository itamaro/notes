---
title: "[Graph/01] Getting the Datasets"
draft: false
tags: 
date: 2024-08-04T11:11:00
---
## Motivation

I want a few DiGraph datasets that look like an "interesting" dependency graph (for some definition of "interesting"), to play around with graph algorithms.

Need a small-enough graph for quick iteration and testing, and a large-enough one for perf to matter, but still small enough to process locally on a single machine.

## OSS dependencies datasets from deps.dev

[deps.dev](https://deps.dev/) is a nice service to query dependencies between OSS packages across multiple ecosystems. They [publish BigQuery datasets](https://docs.deps.dev/bigquery/v1/) that can be used for batch analysis!

Google Cloud docs on using [BigQuery](https://cloud.google.com/bigquery) public datasets: https://cloud.google.com/bigquery/public-data

deps.dev dataset page on the BigQuery datasets marketplace: https://console.cloud.google.com/marketplace/product/bigquery-public-data/deps-dev

I wrote this BigQuery query to get all direct dependency edges for packages in an ecosystem `Sys` (inspired by the "Dependent count" section of the [sample queries](https://docs.deps.dev/bigquery/v1/#sample-queries)):

```sql
WITH

  -- A table with one row per released package+version
  Releases AS (
  SELECT
    Name,
    Version,
    VersionInfo
  FROM
    `bigquery-public-data.deps_dev_v1.PackageVersionsLatest`
  WHERE
    VersionInfo.IsRelease
    AND System = Sys),

  -- A table with one row per released package (the highest released version)
  HighestReleases AS (
  SELECT
    Name,
    Version
  FROM (
    SELECT
      Name,
      Version,
      ROW_NUMBER() OVER (PARTITION BY Name ORDER BY VersionInfo.Ordinal DESC) AS RowNumber
    FROM
      Releases)
  WHERE
    RowNumber = 1 )

SELECT
  D.Name AS package_name,
  D.To.Name AS dep1_name
FROM
  -- The DependencyGraphEdgesLatest table has a complete deps graph (one row per edge), **FOR EVERY PACKAGE**
  -- For example, for package Name="aiohttp", there will be rows for all *recursive dependencies* of `aiohttp` in the `From.*` -> `To.*` columns
  -- So if we want only direct dependencies, we need to keep only the rows where the package `Name`+`Version` is identical to the `From.Name`+`From.Version` columns (the other rows will be *indirect* dep edges)
  `bigquery-public-data.deps_dev_v1.DependencyGraphEdgesLatest` AS D
JOIN
  HighestReleases AS H
ON
  -- Keep only
  H.Name = D.Name
  AND H.Version = D.Version
WHERE
  D.System = Sys
  -- Keep only direct dependency edges
  AND D.From.Name = D.Name
  AND D.From.Version = D.Version
```

[This query](https://console.cloud.google.com/bigquery?sq=964902580044:8a494c69d1c74550a7e2efe3878f92d8) with `Sys='PYPI'` processed 9.94 GB and produced 1,087,123 edges, which is a fairly small graph, for fast loading and iteration when playing with the data later. Since the data is smaller than 1 GB, I could export it as a single file to Google Drive (from the "Save Results" menu in BigQuery), download from Google Drive, and [upload to GitHub](https://github.com/itamaro/fun-with-digraphs/blob/main/data/deps.dev/pypi/bq-20240804-pypi-deps1.csv).

[This query](https://console.cloud.google.com/bigquery?sq=964902580044:3df6bb75a46c414fbc293dffa1dbc148) does the same but with `Sys='NPM'`, which is a significantly larger ecosystem than PyPI. The query processed close to 1 TB (!) and produced 22,699,790 edges, which should be interesting enough for large-ish data analysis (still small enough to do locally, but large enough that performance matters). I failed exporting it to Google Drive (larger than the 1 GB single-file export limit), so I saved the results as a [new BigQuery table](https://console.cloud.google.com/bigquery?ws=!1m5!1m4!4m3!1sonion-on-fire!2sdeps_dot_dev!3snpm-deps1) (from the "Save Results" menu in BigQuery), and then exported *that* to a Google Cloud Storage bucket ([export docs](https://cloud.google.com/bigquery/docs/exporting-data)) by providing a path that ends with a `*` (i.e., `gs://itamaro_depsdev_datasets/npm_deps1/20240804/*`). It created 10 shards of about 105 MB each.

I downloaded the sharded dataset from GCS:

```sh
$ mkdir -p ~/work/digraphs-analysis/fun-with-digraphs/data/deps.dev/npm/20240804
$ cd !$
$ gsutil -m cp \
  "gs://itamaro_depsdev_datasets/npm_deps1/20240804/000000000000" \
  "gs://itamaro_depsdev_datasets/npm_deps1/20240804/000000000001" \
  "gs://itamaro_depsdev_datasets/npm_deps1/20240804/000000000002" \
  "gs://itamaro_depsdev_datasets/npm_deps1/20240804/000000000003" \
  "gs://itamaro_depsdev_datasets/npm_deps1/20240804/000000000004" \
  "gs://itamaro_depsdev_datasets/npm_deps1/20240804/000000000005" \
  "gs://itamaro_depsdev_datasets/npm_deps1/20240804/000000000006" \
  "gs://itamaro_depsdev_datasets/npm_deps1/20240804/000000000007" \
  "gs://itamaro_depsdev_datasets/npm_deps1/20240804/000000000008" \
  "gs://itamaro_depsdev_datasets/npm_deps1/20240804/000000000009" \
  .
$ cd ..
```

Can't upload these as is to GitHub due to GitHub file size limits (files over 100 MB must be GitLFS, which I rather avoid), so I merged and re-sharded them to smaller shards that GitHub can handle (https://github.com/itamaro/fun-with-digraphs/tree/main/data/deps.dev/npm/deps1/20240804):

```sh
# tail -n +2 to strip the csv header from each shard
for shard in 20240804/*; do tail -n +2 "$shard" >> combined.csv; done

mkdir tmp

split -l 543210 -d combined.csv tmp/shard.

# re add the csv header for each shard and add csv suffix
for new_shard in tmp/shard.*; do (echo "package_name,dep1_name"; cat $new_shard) > "$new_shard".csv; done

# move over the new shards and cleanup
rm 20240804/*
mv tmp/*.csv 20240804
rm -r tmp
rm combined.csv
```

## Patent citation network dataset from SNAP

This one is not a "dependency graph" (in the sense of software dependencies), but has a similar structure and might be interesting to play with and compare.

SNAP stands for "Stanford Network Analysis Project" (https://snap.stanford.edu/index.html), and provides [lots of network datasets](https://snap.stanford.edu/data/index.html).

The [Patent citations network dataset](https://snap.stanford.edu/data/cit-Patents.html) contains 3,774,768 patents and 16,518,948 edges that form a directed graph with a node from patent X to Y meaning that patent X cites patent Y.

Like with the npm dataset, the single file dataset exceeds the 100 MB file size limit, so go through a similar sharding exercise. It's also in a space-separated format, so use this opportunity to convert it to CSV. Processed dataset here: https://github.com/itamaro/fun-with-digraphs/tree/main/data/snap/patents-cit

```sh
cd ~/work/digraphs-analysis/fun-with-digraphs/data/snap
mkdir tmp

# clean up the 4-lines header and convert space-separated to comma-separated
tail -n +5 patents-cit/cit-patents.ssv | awk '{print $1 "," $2}' | split -l 1378900 -d - tmp/shard.

# add csv suffix and prepend with an appropriate CSV header
for shard in tmp/shard.*; do (echo "from_patent_id,to_patent_id"; cat $shard) > "$shard".csv; done

# move over the new shards and cleanup
rm patents-cit/cit-patents.ssv
mv tmp/*.csv patents-cit/
rm -r tmp
```

Source: J. Leskovec, J. Kleinberg and C. Faloutsos. [Graphs over Time: Densification Laws, Shrinking Diameters and Possible Explanations](http://www.cs.cmu.edu/~jure/pubs/powergrowth-kdd05.pdf). ACM SIGKDD International Conference on Knowledge Discovery and Data Mining (KDD), 2005.
