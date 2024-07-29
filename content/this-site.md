---
title: How This Site Works
draft: false
tags: 
date: 2024-07-28T20:55:00
---
This site is a hosted on [GitHub Pages](https://docs.github.com/en/pages), backed by [this GitHub repo](https://github.com/itamaro/notes), with content managed using [Obsidian](https://obsidian.md/).

I initially set it up based on [Nicole van der Hoeven](https://nicolevanderhoeven.com/)'s useful [guide](https://notes.nicolevanderhoeven.com/How+to+publish+Obsidian+notes+with+Quartz+on+GitHub+Pages) (ironically, published via Obsidian Publish) and the [Quartz](https://quartz.jzhao.xyz/) docs.

## Workflow

1. The [notes repo](https://github.com/itamaro/notes) is cloned on my Mac.
2. I open the cloned repo in Obsidian as a Vault.
3. I write Markdown files under the [`content/` directory](https://github.com/itamaro/notes/tree/v4/content) using [this template](https://raw.githubusercontent.com/itamaro/notes/v4/templates/note.md) (for the "frontmatter").
4. Run `npx quartz build --serve` in the root directory for local serving / preview.
5. Run `npx quartz sync` in the root directory to push updates to the repo that get auto-published to GitHub Pages (https://itamaro.github.io/notes/).

## Some Implementation Notes

Nicole's guide and the Quartz docs are way more comprehensive and informative. This is "quick notes" for myself.

Quartz is a generic static site generator that has first-class support for Obsidian (i.e. it "understands" Obsidian linking semantics). Install Quartz using `npm` in the cloned repo, and initialize:

```sh
git clone git@github.com:itamaro/notes.git
cd notes
npm i
npx quartz create
```

The site is built and published to GitHub Pages on push using [this GitHub Actions workflow](https://github.com/itamaro/notes/blob/v4/.github/workflows/deploy.yaml) (should probably update it from time to time from [its source of truth](https://quartz.jzhao.xyz/hosting#github-pages)).

The repo needs to have Pages enabled in [the settings](https://github.com/itamaro/notes/settings/pages) with "Source" set to "GitHub Actions".
![[images/20240728-github-pages-repo-settings.png]]

### Configuring Custom Domain

Step 1: Verify the domain following [this guide](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages) (TLDR: add itamaro.com in https://github.com/settings/pages and add the TXT record to the domain DNS).

Step 2: Update the itamaro.com DNS records according to [GitHub's docs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site) (TLDR: add 4 A Records for `@` and `*` hostnames pointing to `185.199.{108..111}.153`, and a CNAME record for `www` hostname pointing to `itamaro.github.io`).

Step 3: Update the [GitHub Pages repo settings](https://github.com/itamaro/notes/settings/pages), setting the "Custom domain" option to "itamaro.com". Wait for all the things to settle (DNS settings, SSL, caches, ...).