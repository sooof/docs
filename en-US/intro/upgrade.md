---
name: 升级指南
sort: 2
---
## openBts Kit Docker映像升级指南
获取最新版本 `wget http://iplink.me/openbts3.09_52M_02_01.tar`


将openbts系统压缩文件转换为本地Docker镜像
```
#cat /media/SYS/openbts3.09_52M_02_01.tar | docker import - openbts3.09_52:0201
```
