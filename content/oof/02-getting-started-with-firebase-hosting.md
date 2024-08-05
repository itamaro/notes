---
title: "[OOF/02] Getting Started With Firebase Hosting"
draft: false
tags:
  - oof
  - firebase
date: 2024-08-01T21:15:00
---
Motivation: [Firebase Hosting](https://firebase.google.com/docs/hosting/) provides free and easy-to-use hosting for static web content, taking care of web serving, CDN, releases and versioning, SSL, routing & URL rewriting (e.g. for SPAs). It integrates well with other components of the Firebase ecosystem, like Authentication, and leveraging Google Cloud Run and Google Cloud Functions to add dynamic content.

Firebase Hosting quickstart: https://firebase.google.com/docs/hosting/quickstart

Install firebase-tools ([docs](https://firebase.google.com/docs/cli#mac-linux-npm))

```sh
$ sudo npm install -g firebase-tools
```

check version

```sh
$ firebase --version
13.15.0
```

authenticate the CLI

```sh
$ firebase login
i  Firebase optionally collects CLI and Emulator Suite usage and error reporting information to help improve our products. Data is collected in accordance with Google's privacy policy (https://policies.google.com/privacy) and is not used to identify you.

? Allow Firebase to collect CLI and Emulator Suite usage and error reporting information? No

Visit this URL on this device to log in:
https://accounts.google.com/o/oauth2/auth?...

Waiting for authentication...

✔  Success! Logged in as ...
```

Initialize a hosting project:

```sh
$ firebase init hosting

     ######## #### ########  ######## ########     ###     ######  ########
     ##        ##  ##     ## ##       ##     ##  ##   ##  ##       ##
     ######    ##  ########  ######   ########  #########  ######  ######
     ##        ##  ##    ##  ##       ##     ## ##     ##       ## ##
     ##       #### ##     ## ######## ########  ##     ##  ######  ########

You're about to initialize a Firebase project in this directory:

  /Users/itamaro/work/oof-web-react


=== Project Setup

First, let's associate this project directory with a Firebase project.
You can create multiple project aliases by running firebase use --add,
but for now we'll just set up a default project.

? Please select an option: Use an existing project
? Select a default Firebase project for this directory: onion-on-fire (Onion)
i  Using project onion-on-fire (Onion)

=== Hosting Setup

Your public directory is the folder (relative to your project directory) that
will contain Hosting assets to be uploaded with firebase deploy. If you
have a build process for your assets, use your build's output directory.

? What do you want to use as your public directory? dist
? Configure as a single-page app (rewrite all urls to /index.html)? Yes
? Set up automatic builds and deploys with GitHub? No
✔  Wrote dist/index.html

i  Writing configuration info to firebase.json...
i  Writing project information to .firebaserc...

✔  Firebase initialization complete!
```

Build the app for deployment:

```sh
$ yarn run build
yarn run v1.22.22
$ vite build
vite v4.5.3 building for production...

... another wall of warnings ...
✓ 1147 modules transformed.
dist/index.html                                             0.57 kB │ gzip:   0.36 kB
dist/assets/logo-19d47add.svg                               0.78 kB │ gzip:   0.54 kB
dist/assets/logo-light-e8abc19b.svg                         0.78 kB │ gzip:   0.54 kB
...
dist/assets/index-090d9158.css                            733.03 kB │ gzip: 124.44 kB
dist/assets/index-ff001fb0.js                           1,399.72 kB │ gzip: 376.78 kB

(!) Some chunks are larger than 500 kBs after minification. Consider:
- Using dynamic import() to code-split the application
- Use build.rollupOptions.output.manualChunks to improve chunking: https://rollupjs.org/configuration-options/#output-manualchunks
- Adjust chunk size limit for this warning via build.chunkSizeWarningLimit.
✓ built in 5.32s
✨  Done in 5.76s.
```

Deploy to Firebase hosting:

```sh
$ firebase deploy --only hosting

=== Deploying to 'onion-on-fire'...

i  deploying hosting
i  hosting[onion-on-fire]: beginning deploy...
i  hosting[onion-on-fire]: found 48 files in dist
✔  hosting[onion-on-fire]: file upload complete
i  hosting[onion-on-fire]: finalizing version...
✔  hosting[onion-on-fire]: version finalized
i  hosting[onion-on-fire]: releasing new version...
✔  hosting[onion-on-fire]: release complete

✔  Deploy complete!

Project Console: https://console.firebase.google.com/project/onion-on-fire/overview
Hosting URL: https://onion-on-fire.web.app
```

Hit https://onion-on-fire.web.app/dashboard to see it in action 🎉

![[../oof/images/20240801-oof-template-firebase-hosting-deployed.png]]