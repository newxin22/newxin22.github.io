---
title : "VTM Compile on Mac OS"
date: 2020-06-11
categories: MacOS, JVET
---


In documentation of VTM(VVC Test Model), for mac os, you can see how to compile with these instruction.<br>

<---->
 **macOS Xcode:**

For generating an Xcode workspace type:

```bash
cd build
cmake .. -G "Xcode"
```
Then open the generated work space in Xcode.

For generating Makefiles with optional non-default compilers, use the following commands:

```bash
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_C_COMPILER=gcc-9 -DCMAKE_CXX_COMPILER=g++-9
```
In this example the brew installed GCC 9 is used for a release build.
<---->

First of all, you can get failure with these instructions due to that in Mac OS, compiler of C(C++) is apple clang(apple clang++).
You can install gcc version 9 manually and these instructions are applied with gcc 9. <br>
I did not install gcc 9 and tried to do cmake the VTM with apple clang compiler. <br>

```
cmake .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_C_COMPILER=/usr/bin/clang -DCMAKE_CXX_COMPILER=/usr/bin/clang++ 
```

and finally did ```make -j```.
