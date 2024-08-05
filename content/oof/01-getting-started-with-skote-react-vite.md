---
title: "[OOF/01] Getting Started With Skote React Vite"
draft: false
tags:
  - oof
  - javascript
date: 2024-08-01T16:08:00
---
Getting started docs: https://themesbrand.com/skote-react/docs/reactjs/getting-started.html

Initialized a new git repo under `~/work/oof-web-react`, and copied in the Skote template startkit (`Skote_React_v4.2.0/Admin_Vite/Starterkit`).

Manually massaged `.gitignore` (present in both git repo template and the Skote template).

Try install deps with yarn and watch it fail:

```sh
$ yarn --version
1.22.22

$ yarn
yarn install v1.22.22
info No lockfile found.
[1/4] 🔍  Resolving packages...
...
[2/4] 🚚  Fetching packages...
warning chart.js@4.4.3: The engine "pnpm" appears to be invalid.
[3/4] 🔗  Linking dependencies...
...
[4/4] 🔨  Building fresh packages...
[-/8] ⡀ waiting...
[-/8] ⡀ waiting...
[3/8] ⡀ node-sass
[-/8] ⡀ waiting...
error /Users/itamaro/work/oof-web-react/node_modules/node-sass: Command failed.
Exit code: 1
Command: node scripts/build.js
Arguments:
Directory: /Users/itamaro/work/oof-web-react/node_modules/node-sass
Output:
Binary found at /Users/itamaro/work/oof-web-react/node_modules/node-sass/vendor/darwin-arm64-115/binding.node
Testing binary
Binary has a problem: Error: dlopen(/Users/itamaro/work/oof-web-react/node_modules/node-sass/vendor/darwin-arm64-115/binding.node, 0x0001): tried: '/Users/itamaro/work/oof-web-react/node_modules/node-sass/vendor/darwin-arm64-115/binding.node' (not a mach-o file), '/System/Volumes/Preboot/Cryptexes/OS/Users/itamaro/work/oof-web-react/node_modules/node-sass/vendor/darwin-arm64-115/binding.node' (no such file), '/Users/itamaro/work/oof-web-react/node_modules/node-sass/vendor/darwin-arm64-115/binding.node' (not a mach-o file)
    at Module._extensions..node (node:internal/modules/cjs/loader:1454:18)
    at Module.load (node:internal/modules/cjs/loader:1208:32)
    at Module._load (node:internal/modules/cjs/loader:1024:12)
    at Module.require (node:internal/modules/cjs/loader:1233:19)
    at require (node:internal/modules/helpers:179:18)
    at module.exports (/Users/itamaro/work/oof-web-react/node_modules/node-sass/lib/binding.js:19:10)
    at Object.<anonymous> (/Users/itamaro/work/oof-web-react/node_modules/node-sass/lib/index.js:13:35)
    at Module._compile (node:internal/modules/cjs/loader:1358:14)
    at Module._extensions..js (node:internal/modules/cjs/loader:1416:10)
    at Module.load (node:internal/modules/cjs/loader:1208:32) {
  code: 'ERR_DLOPEN_FAILED'
}
Building the binary locally
Building: /usr/local/bin/node /Users/itamaro/work/oof-web-react/node_modules/node-gyp/bin/node-gyp.js rebuild --verbose --libsass_ext= --libsass_cflags= --libsass_ldflags= --libsass_library=
gyp info it worked if it ends with ok
gyp verb cli [
gyp verb cli   '/usr/local/bin/node',
gyp verb cli   '/Users/itamaro/work/oof-web-react/node_modules/node-gyp/bin/node-gyp.js',
gyp verb cli   'rebuild',
gyp verb cli   '--verbose',
gyp verb cli   '--libsass_ext=',
gyp verb cli   '--libsass_cflags=',
gyp verb cli   '--libsass_ldflags=',
gyp verb cli   '--libsass_library='
gyp verb cli ]
gyp info using node-gyp@8.4.1
gyp info using node@20.15.1 | darwin | arm64
gyp verb command rebuild []
gyp verb command clean []
gyp verb clean removing "build" directory
gyp verb command configure []
gyp verb find Python Python is not set from command line or npm configuration
gyp verb find Python Python is not set from environment variable PYTHON
gyp verb find Python checking if "python3" can be used
gyp verb find Python - executing "python3" to get executable path
gyp verb find Python - executable path is "/usr/local/bin/python3"
gyp verb find Python - executing "/usr/local/bin/python3" to get version
gyp verb find Python - version is "3.12.4"
gyp info find Python using Python version 3.12.4 found at "/usr/local/bin/python3"
gyp verb get node dir no --target version specified, falling back to host node version: 20.15.1
gyp verb command install [ '20.15.1' ]
gyp verb install input version string "20.15.1"
gyp verb install installing version: 20.15.1
gyp verb install --ensure was passed, so won't reinstall if already installed
gyp verb install version is already installed, need to check "installVersion"
gyp verb got "installVersion" 9
gyp verb needs "installVersion" 9
gyp verb install version is good
gyp verb get node dir target node version installed: 20.15.1
gyp verb build dir attempting to create "build" dir: /Users/itamaro/work/oof-web-react/node_modules/node-sass/build
gyp verb build dir "build" dir needed to be created? Yes
gyp verb build/config.gypi creating config file
gyp verb build/config.gypi writing out config file: /Users/itamaro/work/oof-web-react/node_modules/node-sass/build/config.gypi
gyp verb config.gypi checking for gypi file: /Users/itamaro/work/oof-web-react/node_modules/node-sass/config.gypi
gyp verb common.gypi checking for gypi file: /Users/itamaro/work/oof-web-react/node_modules/node-sass/common.gypi
gyp verb gyp gyp format was not specified; forcing "make"
gyp info spawn /usr/local/bin/python3
gyp info spawn args [
gyp info spawn args   '/Users/itamaro/work/oof-web-react/node_modules/node-gyp/gyp/gyp_main.py',
gyp info spawn args   'binding.gyp',
gyp info spawn args   '-f',
gyp info spawn args   'make',
gyp info spawn args   '-I',
gyp info spawn args   '/Users/itamaro/work/oof-web-react/node_modules/node-sass/build/config.gypi',
gyp info spawn args   '-I',
gyp info spawn args   '/Users/itamaro/work/oof-web-react/node_modules/node-gyp/addon.gypi',
gyp info spawn args   '-I',
gyp info spawn args   '/Users/itamaro/Library/Caches/node-gyp/20.15.1/include/node/common.gypi',
gyp info spawn args   '-Dlibrary=shared_library',
gyp info spawn args   '-Dvisibility=default',
gyp info spawn args   '-Dnode_root_dir=/Users/itamaro/Library/Caches/node-gyp/20.15.1',
gyp info spawn args   '-Dnode_gyp_dir=/Users/itamaro/work/oof-web-react/node_modules/node-gyp',
gyp info spawn args   '-Dnode_lib_file=/Users/itamaro/Library/Caches/node-gyp/20.15.1/<(target_arch)/node.lib',
gyp info spawn args   '-Dmodule_root_dir=/Users/itamaro/work/oof-web-react/node_modules/node-sass',
gyp info spawn args   '-Dnode_engine=v8',
gyp info spawn args   '--depth=.',
gyp info spawn args   '--no-parallel',
gyp info spawn args   '--generator-output',
gyp info spawn args   'build',
gyp info spawn args   '-Goutput_dir=.'
gyp info spawn args ]
Traceback (most recent call last):
  File "/Users/itamaro/work/oof-web-react/node_modules/node-gyp/gyp/gyp_main.py", line 42, in <module>
    import gyp  # noqa: E402
    ^^^^^^^^^^
  File "/Users/itamaro/work/oof-web-react/node_modules/node-gyp/gyp/pylib/gyp/__init__.py", line 9, in <module>
    import gyp.input
  File "/Users/itamaro/work/oof-web-react/node_modules/node-gyp/gyp/pylib/gyp/input.py", line 19, in <module>
    from distutils.version import StrictVersion
ModuleNotFoundError: No module named 'distutils'
gyp ERR! configure error
gyp ERR! stack Error: `gyp` failed with exit code: 1
gyp ERR! stack     at ChildProcess.onCpExit (/Users/itamaro/work/oof-web-react/node_modules/node-gyp/lib/configure.js:259:16)
gyp ERR! stack     at ChildProcess.emit (node:events:519:28)
gyp ERR! stack     at ChildProcess._handle.onexit (node:internal/child_process:294:12)
gyp ERR! System Darwin 23.5.0
gyp ERR! command "/usr/local/bin/node" "/Users/itamaro/work/oof-web-react/node_modules/node-gyp/bin/node-gyp.js" "rebuild" "--verbose" "--libsass_ext=" "--libsass_cflags=" "--libsass_ldflags=" "--libsass_library="
gyp ERR! cwd /Users/itamaro/work/oof-web-react/node_modules/node-sass
gyp ERR! node -v v20.15.1
```

Apparently something named `node-gyp` has a Python script in its build that was not updated to work with Python 3.12 (removed `distutils`). No idea how to fix things in the guts of node deps, so workaround it by getting it to use Python 3.11 instead:

```sh
$ mkdir ~/work/generic_pyenv && cd ~/work/generic_pyenv
$ /Library/Frameworks/Python.framework/Versions/3.11/bin/python3 -m venv py311env
$ source ~/work/generic_pyenv/py311env/bin/activate
$ cd ~/work/oof-web-react
```

This time the install succeeds (with tons of warnings, mostly about deprecations and incorrect "peer deps" 🤷):

```sh
$ yarn
yarn install v1.22.22
info No lockfile found.
[1/4] 🔍  Resolving packages...
...
[2/4] 🚚  Fetching packages...
warning chart.js@4.4.3: The engine "pnpm" appears to be invalid.
[3/4] 🔗  Linking dependencies...
...
[4/4] 🔨  Building fresh packages...
success Saved lockfile.
✨  Done in 78.67s.
```

Commit the generated `yarn.lock` file.

Run dev server:

```sh
$ yarn run dev
yarn run v1.22.22
$vite

  VITE v4.5.3  ready in 286 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h to show help
```

Hit http://localhost:5173/ and behold another wall of warnings (but the page seems to be loading ok).

```sh
Deprecation Warning: Sass's behavior for declarations that appear after nested
rules will be changing to match the behavior specified by CSS in an upcoming
version. To keep the existing behavior, move the declaration above the nested
rule. To opt into the new behavior, wrap the declaration in `& {}`.

More info: https://sass-lang.com/d/mixed-decls

...

Deprecation Warning: Passing percentage units to the global abs() function is deprecated.
In the future, this will emit a CSS abs() function to be resolved by the browser.
To preserve current behavior: math.abs(100%)
To emit a CSS abs() now: abs(#{100%})
More info: https://sass-lang.com/d/abs-percent

...

Warning: 26 repetitive deprecation warnings omitted.
```

But hey, it works..
![[../oof/images/20240801-skote-web-defaults.png]]