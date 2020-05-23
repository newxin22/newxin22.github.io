---
title : "Keras 기반 Multiple Input을 지원하는 뉴럴 네트워크 만들기"
date: 2020-05-23
categories: Keras, Deep Learning, Neural Network
---


현재 Xception을 약간 고쳐서 실험중에 있는데, 이 Xception 을 여섯개를 한꺼번에 붙이고 그 결과물을 FC layer에 보내는 것을 만들어보았다.<br>
즉, 입력값은 이미지 6개인것이고, 출력값은 기냐 아니냐? 하는 것(Sigmoid는 아니고, 2-classes)을 만들자 하는데 목표였다.<br>
찾아보니 Multiple Input은 생각보다 많이 쓰이고 있고, 구글링하면 잘 나오긴 한다.<br>
문제는, 나는 ImageDataGenerator를 사용하여 Augmentation을 하고 있기 때문에 fit_generator 쪽에 수정이 불가피해보이긴 했는데, 찾아보니 ImageDataGenerator쪽을 좀 고치면 된다.<br>

<br><br>
아래 함수는 제너레이터에서 6개 인풋에 배치들을 전달하는 함수다.
<br>

```
def generator_six_imgs(generator, image_dir, batch_size, subset):

    gen = generator.flow_from_directory(directory=image_dir, target_size=(height, width),
                                        batch_size=batch_size, class_mode="categorical", shuffle=True,
                                        subset=subset)
    
    while True:
        gNext = gen.next()
        yield [gNext[0], gNext[0], gNext[0], gNext[0], gNext[0], gNext[0]], gNext[1]
```

아마도 여러 Input에 다같은 이미지 배치들이 들어갈텐데, 일단 이런식으로 하나의 제너레이터로 6개의 인풋을 다 커버치려면 이렇게 추가적인 함수가 필요하다.
그리고 ImageDataGenerator 쪽은 다음과 같이 짰다.

```

generator = ImageDataGenerator(
    rescale=1. / 255,
    horizontal_flip=True,
    vertical_flip=True,
    fill_mode="nearest",
    width_shift_range=0.2,
    height_shift_range=0.2,
    zoom_range=0.2,
    rotation_range=180,
    validation_split=0.2
)

train_generator = generator_six_imgs(
    generator,
    image_dir=train_data_dir,
    batch_size=batch_size,
    subset='training'
)

validation_generator = generator_six_imgs(
    generator,
    image_dir=train_data_dir,
    batch_size=batch_size,
    subset='validation'
)

```

구성한 데이터들을 하나의 디렉토리에 다 쑤셔놓고, validation_split을 설정하여 training과 validation을 나눠주면 된다.<br>
그리고 멀티플 인풋에서 꼭 까먹지 말아야 할 것은, 각각 모델을 다 Model 함수를 이용해서 만들고 나서 Concatenate를 해야 하는 것이다!<br>

```
last_layers1 = model1.output
last_layers1 = GlobalAveragePooling2D()(last_layers1)
customed_model1 = Model(inputs=model1.input, outputs=last_layers1)

last_layers2 = model2.output
last_layers2 = GlobalAveragePooling2D()(last_layers2)
customed_model2 = Model(inputs=model2.input, outputs=last_layers2)

last_layers3 = model3.output
last_layers3 = GlobalAveragePooling2D()(last_layers3)
customed_model3 = Model(inputs=model3.input, outputs=last_layers3)

last_layers4 = model4.output
last_layers4 = GlobalAveragePooling2D()(last_layers4)
customed_model4 = Model(inputs=model4.input, outputs=last_layers4)

last_layers5 = model5.output
last_layers5 = GlobalAveragePooling2D()(last_layers5)
customed_model5 = Model(inputs=model5.input, outputs=last_layers5)

last_layers6 = model6.output
last_layers6 = GlobalAveragePooling2D()(last_layers6)
customed_model6 = Model(inputs=model6.input, outputs=last_layers6)

combined = Concatenate()([customed_model1.output, customed_model2.output, customed_model3.output,
                          customed_model4.output, customed_model5.output, customed_model6.output])
```

이렇게 6개 모델을 일일이 정의하고, Concatenate를 해서! 인풋이 6개인 네트워크를 합쳤고, 이제 아웃풋쪽은 2 클래스 분류기를 작성해줘야 한다.<br>
단순하게 Fully Connected 로 구현했다.<br>

```
finalFullyConnected = Dense(1024, activation="relu")(combined)
finalFullyConnected = Dropout(0.3)(finalFullyConnected)
finalFullyConnected = Dense(num_of_classes, activation="softmax")(finalFullyConnected)

combinedModel = Model(inputs=[customed_model1.input, customed_model2.input, customed_model3.input,
                              customed_model4.input, customed_model5.input, customed_model6.input],
                      outputs=finalFullyConnected)
```

최종적인 모델은 저렇게 Inputs에 구구절절하게 써야하고, 아웃풋은 내가 붙이고 싶었던 FC 써주면 된다.<br>
끗-<br>
