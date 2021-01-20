---
title : "VTM 10.2 Compile on Mac OS with Xcode"
date: 2020-06-11
categories: MacOS, JVET
---


In documentation of VTM(VVC Test Model), for mac os, you can see how to compile with these instruction.<br>

```
bash
cd build
cmake .. -G "Xcode"
```

But, you may receive error that Cmake cannot find compiler :

```
imacsooni@imacsooni build % cmake .. -G "Xcode"
-- The C compiler identification is AppleClang 12.0.0.12000032
-- The CXX compiler identification is AppleClang 12.0.0.12000032
CMake Error at CMakeLists.txt:8 (project):
  No CMAKE_C_COMPILER could be found.



CMake Error at CMakeLists.txt:8 (project):
  No CMAKE_CXX_COMPILER could be found.



-- Configuring incomplete, errors occurred!
See also "/Users/imacsooni/VTM_enable_tracing/10.2/VVCSoftware_VTM-master/build/CMakeFiles/CMakeOutput.log".
```

It was a problem that cmake is not up-to-date, so you can upgrade with brew.

```
brew upgrade cmake
```

After then, compilation will be successful, you can open the project with Xcode.
