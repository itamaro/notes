---
title: Trying to Understand Arch Behavior of Fat Binaries on M3 Mac
draft: false
tags: 
date: 2024-08-05T20:50:00
---
Installing Python 3.12 on macOS using the official python.org-provided macOS universal installer provides a fat python3.12 binary:

```sh
$ file /Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12
/Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12: Mach-O universal binary with 2 architectures: [x86_64:Mach-O 64-bit executable x86_64] [arm64:Mach-O 64-bit executable arm64]
/Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12 (for architecture x86_64):	Mach-O 64-bit executable x86_64
/Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12 (for architecture arm64):	Mach-O 64-bit executable arm64
```

On my M3 MacBook Air, opening iTerm starts zsh running as a native arm64 process:

```sh
$ uname -m
arm64

$ arch
arm64
```

Running Python in that environment defaults to executing the arm64 one, as expected:

```sh
$ python3 -VV
Python 3.12.4 (v3.12.4:8e8a4baf65, Jun  6 2024, 17:33:18) [Clang 13.0.0 (clang-1300.0.29.30)]

$ python3 -c 'import platform; print(platform.machine())'
arm64

$ python3 -c 'import sys; print(sys.executable)'
/usr/local/bin/python3

$ realpath /usr/local/bin/python3
/Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12
```

We can force zsh to run under x86_64 emulation (this simulates a scenario where a parent process provides only x86_64 arch):

```sh
$ arch -x86_64 zsh

$ uname -m
x86_64

$ arch
i386
```

Running Python in the same way as above in this environment will now choose the x86_64 arch from the fat binary:

```sh
$ python3 -c 'import platform; print(platform.machine())'
x86_64

$ python3 -c 'import sys; print(sys.executable)'
/usr/local/bin/python3
```

This is similar to using the `python3.12-intel64` binary (which is a x86_64-only binary, as it says on the tin) to run the universal binary in a subprocess -- the universal binary will choose the x86_64 arch by default:

```sh
$ arch
arm64

$ /Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12-intel64
Python 3.12.4 (v3.12.4:8e8a4baf65, Jun  6 2024, 17:33:18) [Clang 13.0.0 (clang-1300.0.29.30)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import platform
>>> platform.machine()
'x86_64'
>>> import subprocess
>>> subprocess.run(["python3", "-c", "import platform; print(platform.machine())"])
x86_64
CompletedProcess(args=['python3', '-c', 'import platform; print(platform.machine())'], returncode=0)
>>> subprocess.run(["/Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12", "-c", "import platform; print(platform.machine())"])
x86_64
CompletedProcess(args=['/Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12', '-c', 'import platform; print(platform.machine())'], returncode=0)
```

To get the subprocess to choose the arm64 arch from a x86_64 parent process, we need to force it:

```sh
>>> subprocess.run(["arch", "-arm64", "python3", "-c", "import platform; print(platform.machine())"])
arm64
CompletedProcess(args=['arch', '-arm64', 'python3', '-c', 'import platform; print(platform.machine())'], returncode=0)
```