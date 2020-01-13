---
title : "ImportError: cannot import name 'Interpreter'"
date: 2019-08-12
categories: Tensorflow-lite
---

I have been installing tensorflow-lite on raspberry pi 3 for studying edge computing. Installation the tensorflow-lite is so easy.
You can find the installation documentation from here. [Tensorflow-Lite](https://www.tensorflow.org.lite/)

I checked the tensorflow-lite works well but there is a problem. Below is the error I get. 


```
>>>from tflite_runtime import Interpreter
Traceback (most recent call last):
  File "<stdin>", lind 1, in <module>
ImportError: cannot import name 'Interpreter'
```

I just typed code from the tensorflow lite web site and I got the error message.
Because it is just a typing error!
I checked files in tensorflow-lite package(/usr/lib/python3/dist-packages/tflite_runtime), there is no Interpreter.py. Just interpreter.py! 

So I retyped ```from tflite_runtime import interpreter``` and it works well!
