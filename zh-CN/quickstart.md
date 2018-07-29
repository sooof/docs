# 快速入门

## 在 Ubuntu 14.04 安装 Docker
Docker利用Linux容器(LXC)虚拟化技术提供一份部署环境。Docker目的是创建可移植,可分发给任何的Docker环境中运行。Docker由于是OpenVZ的作品,对内核有一些要求，不要在14.04版本的Ubuntu仓库中已经可以查找到。

* 安装Docker使用apt-get命令:

```
$sudo su
```

* 安装Docker使用apt-get命令:

```
#apt-get install docker.io
```

* 启动服务和守护进程

```
#service docker status
#service docker start
```

* 创建软连接

```
#ln -sf /usr/bin/docker.io /usr/local/bin/docker
```

**注意:** 如没有提示错误则说明你已经在Ubuntu14.04上面快速安装Docker成功了。

###### 测试docker：

* 先从中央仓库下一个registry镜像下来

```
#docker pull registry
```

* 下载完成之后可以看到一个￼registry的镜像，通过命令启动容器（需要挂载一个本地目录，防止删除容器时将仓库中的镜像也删掉)

```
#docker run -d -p 5000:5000 -v /opt/data/registry:/tmp/registry registry
```

## 将openbts系统压缩文件转换为本地Docker镜像

 插上U盘，运行以下命令：

```
#cat /media/SYS/openbts3.09_52M_02_01.tar | docker import - openbts3.09_52:0201
```

## 运行openbts3.09_52

*  插上USRP的USB线到笔记上，运行find_usb_dev.sh，找到USB在/dev目录的文件地址。

```
#./find_usb_dev.sh
```


**注：** 脚本文件如下：

  ~~~
  #!/bin/bash   
  lsusb | grep -i "fffe"  | awk '{print $1, $2, $4}' > find_usb_dev.txt
  sed -i "s/://g" find_usb_dev.txt
  sed -i "s/\ /\//g" find_usb_dev.txt
  sed -i "s/Bus/\/dev\/bus\/usb/g" find_usb_dev.txt
  ls  `cat find_usb_dev.txt`
  ~~~

`运行此脚本文件会产生find_usb_dev.txt，此文件里面存放得是USRP的USB设备在/dev目录的文件地址。`

* 运行以下命令登陆Docker openbts3.09_52。

```
#docker run -t -i --device=`cat find_usb_dev.txt`  openbts3.09_52:0201  /bin/bash
```

* 登陆上openbts3.09_52

```
root@9a59ca6ebc6a#cd /usr/local/src/openbts-2.6.0Mamou/apps/
```

* 运行交换asterisk和openbts：

```
root@9a59ca6ebc6a:/usr/local/src/openbts-2.6.0Mamou/apps# asterisk -v &
root@9a59ca6ebc6a:/usr/local/src/openbts-2.6.0Mamou/apps# ./OpenBTS
```
