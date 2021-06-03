---
title : "M1 Apple 컴퓨터에서 opencv 4.5 설치"
date: 2021-06-03
categories: opencv, m1, apple
---


m1으로 바뀌면서 pip install opencv-python 은 계속 에러남.
구글링해보니 다들 cmake 랑 make 로 컴파일 직접하길래 따라 해보니 에러남.
아래 에러는 에러들 중 하나이며, 찾아보니까 컴파일러 때문.
apple-clang 이 기본인데 저 에러 해결하려면 c++ 11인가 쓰라던데..


```
/opt/homebrew/include/ceres/internal/integer_sequence_algorithm.h:136:31: error: no template named 'integer_sequence' in namespace 'std'; did you mean '__integer_sequence'?
std::integer_sequence<T, Rs...>> {
                         
```

굳이 그렇게 까지 하고 싶지 않아서 쿨포기!
혹시나 하고 conda search opencv 해보니 해당 채널에 (https://conda.anaconda.org/conda-forge/osx-arm64) 에 opencv가 있더라.
conda install opencv 로 설치하고 광명 찾음!
