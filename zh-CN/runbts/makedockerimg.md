---
name: 制作Docker映像
sort: 1
---


# 登陆OpenBts映像

## 将openbts系统压缩文件转换为本地Docker镜像

- 下载

```
wget http://iplink.me/openbts3.09_52M_02_01.tar
```
- 运行以下命令：

```
#cat /media/SYS/openbts3.09_52M_02_01.tar | docker import - openbts3.09_52:0201
```
