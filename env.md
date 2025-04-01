---
title: Environment
layout: page
nav_order: 100003
---
# Environment

## Judging Environment

The execution of the contest and the submissions are made on [Vjudge](https://vjudge.net/). However, the judging of the submissions is automatically sent to platforms of the source problem. Then, the result is sent back to Vjudge for score evaluation.

### Types of verdict

Each submission is tested against test cases in the judging server. The judging server may give the following verdicts

| Verdict | Possible Feedback  | Description |
| ------- | -------------- | ----------- |
| <font color="green">AC</font> | Accepted | Your submission have passed all the test cases. Congratulations |
| <font color="red">WA</font> | Wrong Answer | Your submission have failed at least one test case|
| <font color="red">TLE</font> | Time Limit, Time Limit Exceed, Terminated due to timeout | Your submission have exceeded the time limit for at least one test case |
| <font color="red">MLE</font> | Memory Limit, Memory Limit Exceeded | Your submission have exceeded the memory limit for at least one test case |
| <font color="red">RE</font>  | Runtime Error, Segmentation Fault, Abort Called | Your submission have crashed when executing at least one test case |
| <font color="red">CE</font> | Compilation Error | Your submission failed to be compiled |

## Local Environment
### Hardware

Contestants are required to be in the lab to participate in the contest. Contestants write their code using the lab computer, keyboard and mouse. The contestants are not allowed to use their own personal devices during the contests.

### Software

The contests are executed under a controlled environment. Only the following software are allowed. 
1. **Code Editor**: Only the Visual Studio Code editor is allowed to be used during the contest. We allow the contestants to install language extensions such as Python, C/C++, Java to aid their coding. However, generative AI tools such as Copilot are **not** allowed. The contestants are allowed to use terminal only for navigation of files, compilation and execution of code. Please refrain from doing anything else in the terminal that would raise suspicious from the invigilator.
2. **Template code**: Template code (in Python, C, C++ and Java) for each problem are given to all contestants. The template code contains all the necessary libraries, well-formatted input and output, and a class/function which is to be filled by the contestants. The inputs are passed to the class/function through parameters and the contestant is only expected to return the output from the function. The contestant is not required to write the standard input and output. The details of the parameters and return type will be written as comments in the template code. However, the contestant is allowed to modify the template code. The contestants is also allowed to not use the template code. 
3. **Documentation**: We will provide the documentation of the standard libraries for some languages in the local machine. For Python, you cannot use custom libraries (e.g. NumPy, pandas, PyTorch, etc.) unless the problem statement allows you to do so.
    - C/C++: [cppreference](https://en.cppreference.com/)
    - Python: [Python Standard Library](https://docs.python.org/3/library/index.html)
    - Java: [Java Documentation](https://docs.oracle.com/en/java/javase/23/docs/api/)
4. **Compilation**: We installed the compiler for some languages in the local machine. You may use terminal to compile your code. The summary and detailed of the local environment is in the next section.

Remark: We may still allowed more software in future.
### Summary

| Software | Version |
| -------- | ------- |
| [VSCode](https://code.visualstudio.com/) | 1.98.2 |
| [GCC](https://gcc.gnu.org/) | 13.3.0 |
| [Clang](https://clang.llvm.org/) | 11.0.0 |
| [GDB](https://www.gnu.org/software/gdb/) | 14.2 |
| [OpenJDK](https://openjdk.java.net/) | 23.0.2 |
| [CPython](https://www.python.org/) | 3.12.9 |
| [PyPy](https://www.pypy.org/) | 3.11.11 - v7.3.19 |
| [Miniconda](https://www.anaconda.com/docs/getting-started/miniconda/main) | 25.1.1 |
| [Go](https://go.dev/) | 1.24.1 |

### C/C++

#### GCC

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

#### Clang

```sh
$ clang -v
clang version 11.0.0 (https://github.com/msys2/MSYS2-packages 9ef552a3c4cc9410d2b1fb6f22a0cdda3bc09a64)
Target: x86_64-pc-windows-msys
Thread model: posix
InstalledDir: /usr/bin
```

#### GDB

```sh
$ gdb -v
GNU gdb (GDB) 14.2
Copyright (C) 2023 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```

### Java

```sh
> java --version
openjdk 23.0.2 2025-01-21
OpenJDK Runtime Environment (build 23.0.2+7-58)
OpenJDK 64-Bit Server VM (build 23.0.2+7-58, mixed mode, sharing)
```

### Python

#### CPython

```sh
$ python --version
Python 3.12.9
```

#### PyPy

```sh
$ pypy3 --version
Python 3.11.11 (0253c85bf5f8, Feb 26 2025, 10:43:25)
[PyPy 7.3.19 with MSC v.1941 64 bit (AMD64)]
```

#### Miniconda

```sh
$ conda --version
conda 25.1.1
```

### Go

```sh
$ go version
go version go1.24.1 windows/amd64
```
