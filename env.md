---
title: Environment
layout: page
nav_order: 100003
---

# Environment

## Summary

- VSCode: 1.98
- GCC: 13.3.0
- Clang: 11.0.0
- GDB: 14.2
- OpenJDK: 23.0.2
- Python: 3.12.9
- PyPy: 7.3.19
- Miniconda: 25.1.1
- Go: 1.24.1

## C/C++

### GCC

```sh
$ gcc -v
Using built-in specs.
COLLECT_GCC=/usr/bin/gcc
COLLECT_LTO_WRAPPER=/usr/lib/gcc/x86_64-pc-msys/13.3.0/lto-wrapper.exe
Target: x86_64-pc-msys
Configured with: /c/S/B/src/gcc-13.3.0/configure --build=x86_64-pc-msys --prefix=/usr --libexecdir=/usr/lib --enable-bootstrap --enable-static --enable-shared --enable-shared-libgcc --enable-version-specific-runtime-libs --with-arch=nocona --with-tune=generic --disable-multilib --enable-__cxa_atexit --with-dwarf2 --enable-languages=c,c++,lto --enable-graphite --enable-threads=posix --enable-libatomic --enable-libgomp --disable-libitm 
--enable-libquadmath --enable-libquadmath-support --disable-libssp --disable-win32-registry --disable-symvers --with-gnu-ld --with-gnu-as --disable-isl-version-check --enable-checking=release --without-libiconv-prefix --without-libintl-prefix --with-system-zlib --enable-linker-build-id --enable-libstdcxx-filesystem-ts
Thread model: posix
Supported LTO compression algorithms: zlib
gcc version 13.3.0 (GCC)
```

### Clang

```sh
$ clang -v
clang version 11.0.0 (https://github.com/msys2/MSYS2-packages 9ef552a3c4cc9410d2b1fb6f22a0cdda3bc09a64)
Target: x86_64-pc-windows-msys
Thread model: posix
InstalledDir: /usr/bin
```

### GDB

```sh
$ gdb -v
GNU gdb (GDB) 14.2
Copyright (C) 2023 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```

## Java

```sh
> java --version
openjdk 23.0.2 2025-01-21
OpenJDK Runtime Environment (build 23.0.2+7-58)
OpenJDK 64-Bit Server VM (build 23.0.2+7-58, mixed mode, sharing)
```

## Python

### CPython

```sh
$ python --version
Python 3.12.9
```

### PyPy

```sh
$ pypy3 --version
Python 3.11.11 (0253c85bf5f8, Feb 26 2025, 10:43:25)
[PyPy 7.3.19 with MSC v.1941 64 bit (AMD64)]
```

### Miniconda

```sh
$ conda --version
conda 25.1.1
```

## Go

```sh
$ go version
go version go1.24.1 windows/amd64
```
