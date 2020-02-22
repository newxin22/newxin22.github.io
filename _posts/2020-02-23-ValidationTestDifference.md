---
title : "Test Set, Validation Set 차이점?"
date: 2020-02-23
categories: deeplearning, Keras, AI
---


나는 Keras 로 이미지 분류 실험을 진행하고 있다. <br>
Keras 처음 접할 때 트레이닝 할 때 데이터를 어떻게 분할을 할 것이냐에 대해 구글링 해보니... 분명 Training, Validation, Test 라고 나와있었고 구글링하면 Ratio를 3: 1: 1 로 하라고 나와 있었다.<br>
그렇군 하고 현재까지 3: 1: 1 로 트레이닝 했는데, 일단 Keras 쓸 때는 Training, Test 밖에 없다는 걸 최근에 케라스 사이트에서 파라미트르 확인한 결과 알게 되었다.<br>
누가 도대체 3: 1: 1 로 나누라고 한 것인가!!!(빠직) 쨌든 논문 준비하기 이전에 알게 된 부분에 대해 적기로 했다. 안 적으면 금방 까먹는당.<br><br>


<h2>ImageDataGenerator 사용시 Validation_split 매개변수 사용할 때</h2>
먼저 각 데이터 세트에 대한 정의를 보면 다음과 같다<br>
Training Set : 훈련을 위한 데이터 세트, Validation Set : 하이퍼파라미터(Learning Rate, Batch Size and etc.)를 위한 데이터 세트, Test Set : 모델을 평가하는 데이터 세트 <br>
나는 양이 적은 이미지 데이터에 대해 트레이닝을 진행하고 있기 때문에, 케라스에서 제공하는 ImageDataGenerator를 사용하여 이미지 양을 늘렸다.<br>
ImageDataGenerator를 사용하는 경우 Fit 함수는 사용할 수 없고, Fit_generator 함수를 이용하여 트레이닝을 진행 할 수 있다. 이 부분은 케라스 도큐멘테이션만 봐도 금방 알 수 있으니 패스를 하겠다.<br>
여기서 벌어지는 문제는, 흔히 착각할 수 있는(나만 해당할 수 있다..또륵) 네이밍 센스때문이다. Validation_split 이니 당연히 Validation Set 이겠군 생각하고 Validation_split=0.3 정도 설정하고 사용했는데, 문득 이게 잘 돌아가고 있나 하는 생각이 드는것이었다.<br>
분명, Validation Set은 하이퍼 파라미터(Learning Rate, Batch Size and etc.)를 조절할 때 사용하는 세트라고 나와있는데, 안에 코드나 자료구조를 보면 Validation Set이 아니라 Test Set 인 것 같은데.. 도큐멘테이션에도 Validation Data는 트레이닝에 관여를 하지 않고, 모델을 평가/정확도 측정 을 한다는 것이다.<br>
그렇다.<br>
<b>케라스 내에서 Validation 은 그 Validation이 아니고 Test에 해당한다.</b> <br>
Training, Validation(=Test) 라고 보면 된다. <br>
본인이 실험할 때 하이퍼 파라미터 조절은 좀 구닥다리 식으로 진행했었다. Training/Test 에 대해 훈련 및 모델 평가를 하여 게중 Test에 대한 분류 정확도가 가장 높은 하이퍼파라미터를 채택하는 식으로 하였다.<br>
사실 Validation Set이 따로 있는 것은 아니지만 결과적으로 하이퍼파라미터를 조절하는 것은 맞게 된다. <br>
그리고 구글링 하면 100중 99는 Training, Validation, Test에 대해 정의는 잘 알지만, 막상 코드를 짜보면 Validation(하이퍼 파라미터 조절을 위한)에 대해 고려를 하지 않는다. <br>
케라스에서 왜!!!!!! Validation 이라고 명명해서 사람을 헷갈리게 만드는가!!! <br><br>
추후 ImageDataGenerator 를 이용해서 Training, Test 를 만들고 싶다면 다음과 같은 코드를 

```
train_datagen = ImageDataGenerator(
    rescale=1. / 255,
    horizontal_flip=True,
    vertical_flip=True,
    fill_mode="nearest",
    width_shift_range=0.2,
    height_shift_range=0.2,
    zoom_range=0.2,
    rotation_range=180,
    validation_split=0.3
)

train_generator = train_datagen.flow_from_directory(
    train_data_dir,
    target_size=(height, width),
    batch_size=batch_size,
    classes=None,
    class_mode="categorical",
    shuffle=True,
    subset="training"
)
validation_generator = train_datagen.flow_from_directory(
    train_data_dir,
    target_size=(height, width),
    batch_size=batch_size,
    classes=None,
    class_mode="categorical",
    shuffle=True,
    subset="validation"
)
```

subset 은 ImageDataGenerator의 validation_split이 설정이 되어있는 경우에 training 또는 validation 서브셋을 설정을 해줘야 한다.<br>
이러면 또 training, test는 몇대 몇으로 나눠줘야 하나? 생각이 들 수 있는데 7대3 또는 8대2 정도가 적당하다.<br><br>

추가적으로 적은 데이터에 대한 검증 방식인 cross-validation 또한 결국 training/test에 대해 검증하는 방법이고 training/test 가 k-fold 방식으로 바뀐다고 생각하는게 이해하기 편하다.<br>
그런데 요즘은 데이터 증량하는 알고리즘은 참 많다. 여러 transformation 뿐만 아니라 cutout 등 많다. <br> 오버피팅의 문제점을 생각하면 작은 데이터는 오버피팅이 생길 수 있다는 것이기 때문에 작은 양의 데이터로 교차검증하는 것 보단 데이터를 늘리는 것이 조흘 듯 하다.
분노의 작성 끝!<br>



