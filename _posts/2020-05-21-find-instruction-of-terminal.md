---
title : "터미널에서 특정 확장자 옮기기/지우기 명령어"
date: 2020-05-21
categories: MacOS, Terminal, find instruction
---


1. 터미널에서 특정 확장자(예:TXT 파일) 복사하기
```
 find . -type f -name "*.txt" -exec mv {} 옮기는 경로 \;
```

2. 터미널에서 특정 확장자(예: HEIC 이미지) 지우기

```
find . -type f -name "*.heic" -exec rm {} \;
```

터미널이 짱편하다!
