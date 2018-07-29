---
name: 相关命令
sort: 1
---

# 相关命令


```
常用命令
lsusb                  //查看RAD1设备是否与电脑相连  
lsusrp                 //查看gnuradio驱动是否识别RAD1
usrp_probe            //查看RAD1板子的相关信息
usrp_fft.py             //grc的模拟频谱接收模块如：
                      usrp_fft.py –f 900M –R –A
usrp_benchmark_usb.py  //测试RAD1和电脑的USB通信速率

USRP1主板程序刷写
**测试USRP1和电脑的USB通信速率**
/usr/local/share/gnuradio/examples/usrp/usrp_benchmark_usb.py 
** 重新烧写USRP1程序命令**
/usr/local/src/gnuradio-3.3.0/usrp/firmware/src/usrp2/burn-usrp4-eeprom
**以RFX900子板为例，烧写子板  A槽烧写 **
/usr/local/src /gnuradio-3.3.0/usrp/host/apps/burn-db-eeprom -t rfx900_mimo_b -A –f
**以RFX900子板为例，烧写子板  B槽烧写 **
/usr/local/src /gnuradio-3.3.0/usrp/host/apps/burn-db-eeprom -t rfx900_mimo_b -B –f

USRP1测试
USRP1与电脑的USB总线测试
/usr/local/share/gnuradio/examples/usrp/usrp_benchmark_usb.py
benchmark信号发射测试
cd /usr/local/share/gnuradio/examples/digital/narrowband/
./benchmark_tx.py –f 900M –T A    //发射一个900M的信号（使用RFX900子板）

接收测试
cd /usr/local/share/gnuradio/examples/digital/narrowband/
./benchmark_rx.py –f 900M –R A    //接收一个900M的信号（使用RFX900子板）
或者
usrp_fft.py –f 900M –f –R A   //模拟频谱接收900M的信号 （使用RFX900子板）

```
