---
name: docker 安装
sort: 2
---

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

1. 先从中央仓库下一个registry镜像下来

 ```
 #docker pull registry
 ```

2. 下载完成之后可以看到一个￼registry的镜像，通过命令启动容器（需要挂载一个本地目录，防止删除容器时将仓库中的镜像也删掉)

  ```
  #docker run -d -p 5000:5000 -v /opt/data/registry:/tmp/registry registry
  ```


# install_ubuntu1604_docker
