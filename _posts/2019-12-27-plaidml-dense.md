---
title : "PlaidML 기반 Keras 사용할 때 Dense 추가시 주의할 점"
date: 2019-12-27
categories: AMD, Keras, PlaidML, deeplearning
---


연구 피씨가 모자란 관계로 개인 피씨인 아이맥에 AMD 외장으로 연결하여 PlaidML 의 케라스 기반으로 작은 네트워크를 훈련중에 있다. <br>
남의 논문에서 제시한 네트워크를 실험하고 성능평가를 해야 하기에 구현하려고 하는데.. PlaidML 의 케라스 기반으로 코드 작성할 때는 문제점이 있다. 아래의 코드는 BHCNet 의 구조를 코드로 구현한 것 중 일부이다.<br>
BHCNet 논문은 다음 링크에서 확인할 수 있다.<https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0214587> <br>
추가적으로 PlaidML 의 케라스는 Channels_First 의 이미지 데이터 포맷을 쓰는 것을 명심해야 한다.

~~~
    block = Convolution2D(filters=filter, strides=stride, kernel_size=(1, 3), kernel_initializer='zeros',
                          kernel_regularizer=l2(1e-4), padding="same")(input)

    block=BatchNormalization(axis=channel)(block)
    block = Activation(activation='relu')(block)

    block = Convolution2D(filters=filter, strides=(1, 1), kernel_size=(3, 1), kernel_initializer='zeros',
                          kernel_regularizer=l2(1e-4), padding="same")(block)

    block = BatchNormalization(axis=channel)(block)
    block = Activation(activation='relu')(block)

    block = Convolution2D(filters=filter, strides=(1, 1), kernel_size=(1, 3), kernel_initializer='zeros',
                          kernel_regularizer=l2(1e-4), padding="same")(block)

    block = BatchNormalization(axis=channel)(block)
    block = Activation(activation='relu')(block)

    block = Convolution2D(filters=filter, strides=(1, 1), kernel_size=(3, 1), kernel_initializer='zeros',
                          kernel_regularizer=l2(1e-4), padding="same")(block)

    # Squeeze and Excitation
    se = GlobalAveragePooling2D(data_format='channels_first')(block)

    se = Reshape((filter, 1, 1))(se)

    se = Dense(units=filter // ratio, input_shape=(1, 1, 16), activation='relu', kernel_initializer='he_normal',
               kernel_regularizer=regularizers.l2(1e-4))(se)

    se = Dense(units=filter, activation='sigmoid', kernel_initializer='he_normal',
               kernel_regularizer=regularizers.l2(1e-4))(se)


    
~~~

<br><br>
분명 Dense 파라미터의 units 는 Depth, 즉 채널 수를 의미하는 것인데 이대로 수행하면 다음과 같은 에러를 받을 수 있다.<br><br>

~~~
Traceback (most recent call last):
  File "/Users/imacsooni/PycharmProjects/BHCNet_3.6/bhcnet.py", line 234, in <module>
    model = BHCNet(channel=CHANNEL_AXIS)
  File "/Users/imacsooni/PycharmProjects/BHCNet_3.6/bhcnet.py", line 150, in BHCNet
    bhcnet = small_basic_block(X_input, filter=16, stride=(1, 1), channel=channel)
  File "/Users/imacsooni/PycharmProjects/BHCNet_3.6/bhcnet.py", line 139, in small_basic_block
    x = multiply([block, se])
  File "/Users/imacsooni/BHCNet_3.6/lib/python3.6/site-packages/keras/layers/merge.py", line 596, in multiply
    return Multiply(**kwargs)(inputs)
  File "/Users/imacsooni/BHCNet_3.6/lib/python3.6/site-packages/keras/engine/base_layer.py", line 431, in __call__
    self.build(unpack_singleton(input_shapes))
  File "/Users/imacsooni/BHCNet_3.6/lib/python3.6/site-packages/keras/layers/merge.py", line 91, in build
    shape)
  File "/Users/imacsooni/BHCNet_3.6/lib/python3.6/site-packages/keras/layers/merge.py", line 61, in _compute_elemwise_op_output_shape
    str(shape1) + ' ' + str(shape2))
ValueError: Operands could not be broadcast together with shapes (16, 224, 224) (16, 1, 16)
~~~

두 번째 Dense 레이어에서 채널 수가 바뀌는 것이 아니라 이미지의 개수가 변하는 것을 알 수 있다. 개인적인 의견으로는 버그 같다. <br>
따라서 이 부분에서는 Channel Last로 Permute 해주는 과정이 필요하다<br>

~~~

se = Permute((3, 2, 1))(se)

se = Dense(units=filter // ratio, input_shape=(1, 1, 16), activation='relu', kernel_initializer='he_normal',
               kernel_regularizer=regularizers.l2(1e-4))(se)

se = Dense(units=filter, activation='sigmoid', kernel_initializer='he_normal',
               kernel_regularizer=regularizers.l2(1e-4))(se)
               
se = Permute((3, 2, 1))(se)
~~~

이렇게 퍼뮤트 2번만 넣어줬더니 작동 잘 한다. 근데.. 역시 엔비디아로 실험하는게 짱이다..<br>
같은 데이터로 테스트 하는데 한 에포크당 거의 5분 잡아먹는다. GTX 1080ti 로 실험할 때는 안 에포크당 2분정도인데.. ㅜㅜ<br>
머신 러닝할 때는 엔비디아가 짱이라고 생각하지만 독점적인 모습을 보니 슬프다.
