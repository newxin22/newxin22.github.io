---
title : "기존의 네트워크르 수정하였지만 ImageNet 가중치는 사용하고 싶을때"
date: 2020-01-13
categories: deeplearning, Keras, AI
---


나는 케라스에서 제공하고 있는 네트워크를 수정하고 정확도를 실험하는 중에 있다. <br>
그런데 아무래도 케라스에서 제공하는 네트워크는 헤비하다 보니 개인적으로 실험을 위해 네트워크를 줄여서 실험중에 있는데, 찾아보면 알겠지만 좋은 가중치를 찾는 것도 쉽지 않은 부분이다. <br>
이전에 실험하였을 때는 imageNet 가중치가 가장 잘 나오기도 했고…<br><br>

그래서 케라스 코드 부분중에서 가중치를 불러오는 부분에서 분명 손 델 곳이 있거나 또는 가중치를 직접 다뤄서 고친 후에 실험해봐야겠다 하고 실험을 해봤다.<br>

결론적으로 가중치를 직접 다뤄서 하는 건 생각보다 불편하다.<br>
이유는 소프트웨어가 그렇게 좋지 않기 때문이다.<br>
내가 하는 실험은 네트워크의 중간 부분만 잘라내기 때문에 중간 부분의 가중치를 자르는 식으로 했는데 여간 불편한게 아니었다.<br>

그래서 가중치 불러오는 부분에서 고칠 곳이 있을 것인가 찾아보는데..<br>

/keras/engine/network.py 파일 안에는 load_weights라는 함수가 있다. 이는 가중치를 불러오는 함수인데, imageNet도 결국 가중치를 불러와야 하니까 이 함수를 잘 보니..<br>

```
def load_weights(self, filepath, by_name=False,
                     skip_mismatch=False, reshape=False):
        """Loads all layer weights from a HDF5 save file.

        If `by_name` is False (default) weights are loaded
        based on the network's topology, meaning the architecture
        should be the same as when the weights were saved.
        Note that layers that don't have weights are not taken
        into account in the topological ordering, so adding or
        removing layers is fine as long as they don't have weights.

        If `by_name` is True, weights are loaded into layers
        only if they share the same name. This is useful
        for fine-tuning or transfer-learning models where
        some of the layers have changed.

        # Arguments
            filepath: String, path to the weights file to load.
            by_name: Boolean, whether to load weights by name
                or by topological order.
            skip_mismatch: Boolean, whether to skip loading of layers
                where there is a mismatch in the number of weights,
                or a mismatch in the shape of the weight
                (only valid when `by_name`=True).
            reshape: Reshape weights to fit the layer when the correct number
                of weight arrays is present but their shape does not match.

```
<br>
잘 보면 reshape 부분이 맞지 않을때 가중치를 재구성해준다는 식으로 쓰여있다. 케라스에서 제공하는 값은 False 이지만 나는 실험을 위해 True로 변경하고 실험하니 수정된 네트워크도 imageNet 가중치를 잘 이용할 수 있게 되었다.<br>

<br><br><br>
이 글은 내가 까먹을 까봐 쓴것이다!<br>
