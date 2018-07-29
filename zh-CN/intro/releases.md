---
name: 发布版本
sort: 1
---

# openbts3.09_52M_02_01：
	openbts3.09_52M_02_01.tar ---  openBts3.09_52 压缩文件 Ubuntu 10.10
  1. Mobile Network Code: GSM.MCC == 310
  2. Location Area Code:  GSM.MNC ==  07
  3. GSM.Band== 900
  4. Valid ARFCN range depends on the band.GSM.ARFCN == 73
  5. 基站收:=904.6MHz
  6. 基站发:=949.6MHz

# openbts3.09_52M_01_01：
	system.tar --- 老版本 openBts3.09_52 压缩文件 Ubuntu 10.10
  1. Mobile Network Code: GSM.MCC == 460
  2. Location Area Code:  GSM.MNC ==  07
  3. GSM.Band== 900
  4. Valid ARFCN range depends on the band.GSM.ARFCN == 73
  5. 基站收:=904.6MHz
  6. 基站发:=949.6MHz
  6. Mobile Network Code: GSM.MCC ==






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

  ## 将openbts系统压缩文件转换为本地Docker镜像

   插上U盘，运行以下命令：
     ```
     #cat /media/SYS/openbts3.09_52M_02_01.tar | docker import - openbts3.09_52:0201
     ```

  ## 运行openbts3.09_52

  1. 插上USRP的USB线到笔记上，运行find_usb_dev.sh，找到USB在/dev目录的文件地址。
     ```
     #./find_usb_dev.sh
     ```
     **注：** 脚本文件如下：
     ```
     #!/bin/bash   

     lsusb | grep -i "fffe"  | awk '{print $1, $2, $4}' > find_usb_dev.txt
     sed -i "s/://g" find_usb_dev.txt
     sed -i "s/\ /\//g" find_usb_dev.txt
     sed -i "s/Bus/\/dev\/bus\/usb/g" find_usb_dev.txt
     ls -al  `cat find_usb_dev.txt`
     ```
     运行此脚本文件会产生find_usb_dev.txt，此文件里面存放得是USRP的USB设备在/dev目录的文件地址。
  2. 运行以下命令登陆Docker openbts3.09_52。

    ```
    #docker run -t -i --device=`cat find_usb_dev.txt`  openbts3.09_52:0201  /bin/bash
    ```

  3. 登陆上openbts3.09_52
     ```
     root@9a59ca6ebc6a#cd /usr/local/src/openbts-2.6.0Mamou/apps/
     ```
     首先运行：
     ```
     root@9a59ca6ebc6a:/usr/local/src/openbts-2.6.0Mamou/apps# asterisk -v &
     root@9a59ca6ebc6a:/usr/local/src/openbts-2.6.0Mamou/apps# ./OpenBTS
     ```


     # openbts3.09_52M_02_01.tar 系统压缩文件 的说明

     openbts3.09_52M_02_01.tar文件配置环境

    * Ubuntu 10.10系统。
    * gnuradio-3.3.0
    * openbts-2.6.0Mamou
    * Asterisk 1.6.2.7
    * kal-0.3 --- Kal扫描GSM基站
    * 源码存放位置/usr/local/src/
    * asterisk 配置文件存放地址 /etc/asterisk/
        * asterisk 主要需配置两个文件：extensions.conf sip.conf
        * 配置完成后需要重新加载配置，运行的命令如下：
        ```
        #asterisk -rx "sip reload"
        #asterisk -rx "dialplan reload"
        ```

     Openbts的主要配置参数是：
     ```
     Mobile Network Code: GSM.MCC == 310
     Location Area Code:  GSM.MNC ==  07
                        GSM.Band== 900
     Valid ARFCN range depends on the band.GSM.ARFCN == 73
     ```




  # （原） 关于Openbts liveDVD运行说明：
  OpenbtsV3.09光盘

  ## 此光盘的软件配置为：
  * gnuradio-3.3.0
  * openbts-2.6Mamou
  * asterisk1.6.2.7
  * kal-0.3
  * ortp-0.16.1
  * libosip2-3.6.0
  * qwt-6.0.1
  * swig-2.0.4

  ## 运行openbts
  1. 首先将电脑设为光驱启动，并Openbts liveDVD放入光驱，启动电源
  2. 选Try Ubuntu
  3. 进入桌面
     选屏幕顶层的任务栏的
     Application--->Accessories--->Terminal
     打开4个Terminal
     在Terminal 1 输入：
     ## 此光盘的软件配置为
     ```
     sudo su
     cd /usr/local/src/openbts-2.6Mamou/apps
     tail -f TRX26.log
     ```
     【查看日志】
     在Terminal 2 输入：
     ```
     sudo su
     cd /usr/local/src/openbts-2.6Mamou/apps
     tail -f openbts26.log
     ```
     【查看日志】
     在Terminal 3 输入：
     ```
     sudo su
     cd /usr/local/src/openbts-2.6Mamou/smqueue
     ./smqueue
     ```
     【运行短信功能】
     在Terminal 4 输入：
     ```
     sudo su
     cd /usr/local/src/openbts-2.6Mamou/apps
     ./Openbts
     ```
     会显示如下内容：
     ```
     Starting the system...
     1330501158.8974 ALARM 3079042768 OpenBTS.cpp:129:main: OpenBTS starting, ver 2.6PUBLIC build date Feb 26 2012
     1330501158.9037 FORCE 3078567632 Logger.cpp:194:gLogInit: Setting initial global logging level to INFO
     1330501164.0128 ALARM 3078654832 Transceiver.cpp:551:driveControl: TX failed to tune
     1330501164.0128 ALARM 3079042768 TRXManager.cpp:349:tune: TXTUNE failed with status 1
     1330501164.495934 3079042768:
     Welcome to OpenBTS.  Type "help" to see available commands.
     OpenBTS>
     ```
     【运行OPENBTS】
     新打开一个Terminal 5输入：
     ```
     sudo su
     asterisk -rx "sip reload"
     asterisk -rx "dialplan reload"
     ```
     【重载asterisk的关于openbts的配置文件】
     然后运行手机手动搜索运营商
     搜索倒310 07（310 07为此光盘所设定的基站）
     连接
