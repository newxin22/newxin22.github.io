---
title : "Version checking for deep learning"
date: 2019-11-18
categories: Tensorflow, Keras, Cuda, Deep Learning
---


This is just record for me because version checking is confusing.

Now, Using CentOS 7, (Kernel 3.10) and GTX 1080 ti.
For deep learning installation of Nvidia Driver and Cuda toolkit and Cudnn is required or mandatory.
First I installed Nvidia Driver version 390.XX and Cuda Toolkit 10.0. (Cudnn version is set according to Cuda toolkit version)
because I saw  somewhere that tensorflow 1.14 not working with Cuda Toolkit 10.1.

Tensorflow 2.0 is faster but I think package manager in pycharm may not support tensorflow 2.0 so utilize 1.14 version.
After installation of everything was done, I tried to retrain weights of Xception for test but I received error msg that Cuda driver version is insufficient.
But it is not problem about Cuda version, but Nvidia driver version. 
In tensorflow documentation, nvidia driver verison is required 410.XX or higher.
So I reinstalled only Nvidia driver version 418.xx and tried to retrain Xception and then it was working with GPU.


In addition, after installation of cuda toolkit, cudnn, linking cuda/cudnn libraries is necessary as followed.

```
sudo ldconfig /usr/local/cuda-10.0/lib64

```

if you not linked the libraries, GPU is not utilized for training.
