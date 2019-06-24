---
title : "failed to initialize nvml driver/library version mismatch 문제 발생"
date: 2019-06-24
categories: Nvidia
---

Centos 7 사용중인데, 재부팅하니 그래픽이 좀 깨지고 계속 로그아웃 되길래 쎄한 느낌에 ```nvidia-smi``` 명령어를 쳤더니 아래와 같은 에러 메세지가 떡 하니 떴다.

Failed to initialize nvml driver/library version mismatch



구글링 해보니 Nvidia 커널 모듈 언로딩 & 로딩 하라고 나와있길래 그대로 해보니 똑같은 에러가 발생했다. [NVML:Driver/library mismatch 에러 해결](https://medium.com/@jjeaby/nvml-driver-library-version-mismatch-문제-해결-e84047a30a8c). 

```dmesg```

로 에러 메세지를 상세히 확인해본 에러 메세지는 아래와 같다.

NVRM : API mismatch : the client has the version 418.74, but this kernel module has the version 418.67. Please make sure that this kernel module and all NVIDIA driver

그래서 Nvidia Driver 418.74로 드라이버 업데이트 해줬더니 잘 작동한다.
