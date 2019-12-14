---
title : "Teamviewer 원격으로 비밀번호 변경하기"
date: 2019-12-14
categories: Teamviewer
---

CentOS 에서 머신 러닝 실험하는데 평소 PyCharm을 사용하기 때문에 GUI 원격 접속이 필요하다. 물론 노트북 세팅하면 원격으로 하기 더 편하긴 할텐데, 귀찮아서 안하고 팀뷰어를 설치하여 사용하고 있다.
CentOS는 너무 연약한 아이라서 그런지 평소 재부팅만 빡세개 하면 파일이 잘 깨지고.. 아무 생각없이 파일 좀 옮겼다고 갑자기 팀뷰어도 접속이 안되고..

그래서 팀뷰어를 재시작하고 프로세스 찾아보고 킬9를 하였다.

```
[xintos@localhost ~]$ ps -ef | grep team
xintos   11603     1  0 01:14 ?        00:00:00 /opt/teamviewer/tv_bin/TeamViewer
root     11649     1  0 01:14 ?        00:00:01 /opt/teamviewer/tv_bin/teamviewerd -d
xintos   11981 11406  0 01:19 pts/4    00:00:00 grep --color=auto team
```

재시작은 teamviewer& 로 켰는데 접속하려니까 비밀번호가 바뀌어 있었다.
그래서 원격으로 비밀번호를 변경해야 했다. 그래서 다음과 같이 비밀번호를 변경하였다.

```
[xintos@localhost ~]$ teamviewer info

 TeamViewer                           14.7.1965  (RPM) 

 TeamViewer ID:                        1680132084 

 teamviewerd status                   ● teamviewerd.service - TeamViewer remote control daemon
   Loaded: loaded (/etc/systemd/system/teamviewerd.service; enabled; vendor preset: disabled)
   Active: active (running) since 토 2019-12-14 00:59:37 EST; 14min ago
  Process: 10118 ExecStart=/opt/teamviewer/tv_bin/teamviewerd -d (code=exited, status=0/SUCCESS)
 Main PID: 10122 (teamviewerd)
    Tasks: 24
   CGroup: /system.slice/teamviewerd.service
           └─10122 /opt/teamviewer/tv_bin/teamviewerd -d 

[xintos@localhost ~]$ sudo teamviewer passwd rharnr12
[sudo] xintos의 암호: 

ok


[xintos@localhost ~]$ teamviewer info

 TeamViewer                           14.7.1965  (RPM) 

 TeamViewer ID:                        1680132084 

 teamviewerd status                   ● teamviewerd.service - TeamViewer remote control daemon
   Loaded: loaded (/etc/systemd/system/teamviewerd.service; enabled; vendor preset: disabled)
   Active: active (running) since 토 2019-12-14 01:14:10 EST; 4s ago
  Process: 11645 ExecStart=/opt/teamviewer/tv_bin/teamviewerd -d (code=exited, status=0/SUCCESS)
 Main PID: 11649 (teamviewerd)
    Tasks: 24
   CGroup: /system.slice/teamviewerd.service
           └─11649 /opt/teamviewer/tv_bin/teamviewerd -d 
```

그래서 접속은 잘 되는데.. 왜 파라미터 초기화중에서 멈추니.. 또륵...
