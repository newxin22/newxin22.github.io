---
title : "Keras Multi GPU Setup"
date: 2020-01-02
categories: deeplearning, Keras
---

This time I have a chance to get two RTX 2080 ti so I decided to train with these GPUs.<br>
Thesedays, I always train my network which is extended from Xception network so I do not need to type new codes related to deep learning. <br>
And I have to set up for multi gpus. My develop environment is Keras, Tensorflow and Ubuntu. So you can find online how to install of Keras, Tensorflow and even for cuda, cudnn.<br> 
<br>
<br>

For utilizing multi-gpu, you can type simply like this:

```
from keras.util.mutl_gpu_utils import multi_gpu_model
```

and you can type code related to multi gpu. gpus of parameter means number of gpus. I have 2 gpus so I give 2 value to parameter.

```
model.summary()
plot_model(model, to_file='model.png')
model = multi_gpu_model(model, gpus=2)
model.compile(loss='categorical_crossentropy', optimizer=sgd, metrics=['acc'])
```
<br>
It's done! So easy! And then you can train your networks with multiple gpus and you can check usage of gpu with nvidis-smi.<br>

```
xxx@xxx:~$ nvidia-smi
Thu Jan  2 06:22:52 2020       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 440.44       Driver Version: 440.44       CUDA Version: 10.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce RTX 208...  Off  | 00000000:01:00.0 Off |                  N/A |
| 52%   79C    P2   105W / 300W |  10966MiB / 11019MiB |     43%      Default |
+-------------------------------+----------------------+----------------------+
|   1  GeForce RTX 208...  Off  | 00000000:02:00.0 Off |                  N/A |
| 31%   65C    P2    88W / 300W |  10965MiB / 11019MiB |     44%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      1012      G   /usr/lib/xorg/Xorg                             9MiB |
|    0      1093      G   /usr/bin/gnome-shell                          14MiB |
|    0     17773      C   python3                                    10929MiB |
|    1     17773      C   python3                                    10953MiB |
+-----------------------------------------------------------------------------+
```
