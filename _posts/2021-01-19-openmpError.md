---
title : "Detectron2 with only cpu got error related to openmp"
date: 2021-01-19
categories: MacOS, Detectron2
---


I tested detectron2 (only cpu, NVIDIA does not support CUDA for mac os) and received error.

```
OMP: Error #15: Initializing libiomp5.dylib, but found libomp.dylib already initialized.
OMP: Hint This means that multiple copies of the OpenMP runtime have been linked into the program. That is dangerous, since it can degrade performance or cause incorrect results. The best thing to do is to ensure that only a single OpenMP runtime is linked into the process, e.g. by avoiding static linking of the OpenMP runtime in any library. As an unsafe, unsupported, undocumented workaround you can set the environment variable KMP_DUPLICATE_LIB_OK=TRUE to allow the program to continue to execute, but that may cause crashes or silently produce incorrect results. For more information, please see http://www.intel.com/software/products/support/.
```

It is an error related to open mp and for solving you should type instruction on the top of code.

```
os.environ[‘KMP_DUPLICATE_LIB_OK’]=’True’
```
