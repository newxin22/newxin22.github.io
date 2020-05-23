---
title : "Ubuntu 18.04 네트워크 세팅"
date: 2020-05-24
categories: Ubuntu, Network Setup
---


우분투 18.04 네트워크 세팅<br>
Ping이 안날아가거나 (Name or service not known) apt 로 뭐 설치하려는데 서버 못 찾을때<br>
/etc/systemd/resolved.conf 에서 Nameserver 고치고 systemctl restart systemd-resolved 명령어 치면 끝-!<br>
네트워크 잼병은 맨날 헷갈리는것~!
