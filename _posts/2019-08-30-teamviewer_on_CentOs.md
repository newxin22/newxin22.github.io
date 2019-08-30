---
title : "Teamviewer launching failure on CentOS 7"
date: 2019-08-12
categories: CentOS, Teamviewer
---

I tried installation of teamviewer on CentOS 7 for remote control but when typing teamviewer on the terminal it did not launched.
So I typed these commands and got error message like this.

```
[xintos@xintos ~]$ cat /var/log/teamviewer14/xintos/gui.log 
/opt/teamviewer/tv_bin/TeamViewer: error while loading shared libraries: libQt5Sensors.so.5: cannot open shared object file: No such file or directory
```

The problem was related to QT library and I searched releted libraries with command 'yum search qt5-qt'
There are many libraries about QT library. So I installed qt5-qtsensors with yum. After that, retyped teamviewer but I got another errors.

```
[xintos@xintos ~]$ cat /var/log/teamviewer14/xintos/gui.log 
/opt/teamviewer/tv_bin/TeamViewer: error while loading shared libraries: libQt5Positioning.so.5: cannot open shared object file: No such file or directory
```

Another library named libQT5Positioning have to be installed!!! In CentOS 7 you can not find libQT5Positioning with yum search but there are renamed libraries something like that.

```
qt5-qtlocation-devel.i686 : Development files for qt5-qtlocation
qt5-qtlocation-devel.x86_64 : Development files for qt5-qtlocation
qt5-qtlocation-doc.noarch : API documentation for qt5-qtlocation
qt5-qtlocation-examples.x86_64 : Programming examples for qt5-qtlocation
```

It just changed library name so you can install libQT5Positioning with this command on the terminal.

```
yum install qt5-qtlocation 
```

After that another problem occurs and it is very annoying because similiar problem occurs in succession like this.

```
[xintos@xintos ~]$ cat /var/log/teamviewer14/xintos/gui.log 
/opt/teamviewer/tv_bin/TeamViewer: error while loading shared libraries: libQt5WebChannel.so.5: cannot open shared object file: No such file or directory
```

You can fix the problem yourself now because the problem is related to QT library. You can install qt5-qtwebchannel with yum.
I think that vncserver is worse than teamviewer because configuration of vncserver(tigervnc-server) is too difficult. However launching failure of teamviewer is a problem that libraries not be installed.
