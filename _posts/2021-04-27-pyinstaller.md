---
title : "Pyinstaller 에서 __import__ 가 작동아 안된다면"
date: 2021-04-27
categories: Pyinstaller
---


프로젝트 진행중에 OpenCV, Numpy 가 필요한 부분이 있어 import 하고 Pyinstaller 쓰니까 실행파일 크기가 369메가인가 그랬다.
너무 커서 OpenCV, Numpy 를 빼려는데 한 방법이 main 중에 라이브러리르 import 하는 방법이었다.
해당 방법은 sys.path.append() 하고 __import()__ 함수를 쓰면 된다.
그런데, sys.path.append()는 list 의 맨 마지막에 경로를 넣어주는 함수인데, 쓰고 싶은 라이브러리를 우선으로 하고 싶다면, 
sys.path.insert(0, "path")로 하면 된다.

암튼, 원래는 라이브러리 경로 알려주고, __import__ 하면 될 줄 알았는데 안 되는 것이다! 
쒹쒹 거리고 있는데, 알고보니 pyinstaller 쓸때 파이썬 버전도 중요하다!
3.6으로 하면 경로 못 찾는데, 3.8로 하니 경로 잘 찾는다.
그러므로 pyinstaller 로 실행파일 만들때 동적으로 라이브러리를 임포트 하고 싶다면 3.8을 쓰자!
