---
title : "When accuracy doen not change using Keras"
date: 2020-02-23
categories: deeplearning, Keras, AI
---


I'm doing experiments my deep neural network using Keras for classification test using. <br>
Sometimes, when tests, accuracy/validation accuracy does not change due to some reasons. Many people say that change learning rate lower because high learning rate can not improve the accuracy<br>
My data set is small and have 3-more classes, so even though lowering the learning rate, accuracy can not be improved.<br>
Then, with small and several classes, batch size must be smaller than default value. <br>
I trained nn with batch size 16 before, accuracy does not improve. So I set the batch size 12, thereupon accuracy is improving. <br>
In results, when accuracy not chaning, you should lower learning rate and also batch size. 
