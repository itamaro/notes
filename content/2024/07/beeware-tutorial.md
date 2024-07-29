---
title: "BeeWare: Desktop + Mobile Cross Platform Python Apps!"
draft: false
tags:
  - python
date: 2024-07-14T10:50:00
---
[Russell Keith-Magee](https://beeware.org/community/team/freakboy3742/) gave a great [tutorial](https://youtu.be/New2JLvWxiE) (and talk) at PyCon US 2024, working through using [BeeWare](https://beeware.org/) tools ([Briefcase](https://beeware.org/project/projects/tools/briefcase/) and [Toga](https://beeware.org/project/projects/libraries/toga/)) to build a simple Python app and packaging and running it *natively* (from a single codebase!) for multiple target platforms, including desktop and mobile.

Briefcase handles the packaging for the target platform (i.e. it can produce a macOS dmg installer and an Android aab/apk), and Toga is an OS-native GUI toolkit that uses native UI components for the target platform so the app feels and looks natural. This means Toga isn't a great choice when pixel-perfect control is necessary (i.e. graphical games), but it's perfect for lots of other things (i.e. business-logic-centric use cases).

I watched the recording and followed along the tutorial: https://github.com/itamaro/beeware-tutorial-toga

Here's a screenshot of the installed app running natively on my ARM-based MacBook (including the menu bar, demonstrating native behaviors for macOS):
![[../../images/2024-07-13-beeware-hello-world-macos.png]]

My tutorial terminal log:

```sh
python3 -m venv beewarenv
source beewarenv/bin/activate
python -m pip install briefcase
briefcase --version
briefcase new
cd helloworld
briefcase dev -vv

## macOS app
briefcase create
briefcase build
briefcase run
briefcase package --adhoc-sign

## update or run and update
briefcase update
briefcase run -u
briefcase update --update-resources

## android
briefcase create android
briefcase build android
briefcase run android --update-resources
briefcase package android --adhoc-sign

## web
briefcase create web
briefcase build web
briefcase run web -u

## test
briefcase dev --test -r
briefcase run Android --test -r
```