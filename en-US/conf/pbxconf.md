---
name: 软交换配置
sort: 2
---

# 软交换配置

配置Asterisk：

A．    将OpenBTS中AsteriskConfigure/文件夹下的sip.conf和extensions.conf的内容复制到/etc/asterisk/下sip.conf和extensions.conf。若使用自动注册用户程序，则不用对sip.conf和extensions.conf文件进行编辑。

B．    若使用手动注册，测、、则需修改sip.conf和extensions.conf文件，过程如下：

1)         得到SIM卡的IMSI号

通过OpenBTS自身得到IMSI号，当手机试图接入OpenBTS时，会向OpenBTS发送IMSI号，可通过OpenBTS的CLI显示的信息中读出，如下所示：

RadioResource.cpp:152: AccessGrantResponder RA=0x15 when=0:1192710 age=25 TOA=0.0000

ChannelDescription=(typeAndOffset=SDCCH/4-0 TN=0 TSC=0 ARFCN=975) RequestReference=(RA=21 T1'=3 T2=12 T3=24) TimingAdvance=0

MobilityManagement.cpp:119: LocationUpdatingController MM Location Updating Request  LAI=(MCC=901 MNC=55 LAC=0x29b) MobileIdentity=(TMSI=0x49ffcddd)

MobilityManagement.cpp:172: LocationUpdatingController registration FAIL: IMSI=234100223456161

2)         把上一步得到的IMSI号写入sip.conf中，示例如下：

[IMSI460004311159502] ; 460004311159502为IMSI号

callerid=IMSI460004311159502 <2102>;<>内为该手机的号码

canreinvite=no

type=friend

allow=gsm

context=sip-external

host=dynamic

3)         在extensions.conf中[ sip-local ]中加入如下语句：

; This is a simple mapping between extensions and IMSIs.

exten => 2102,1,Macro(dialSIP,IMSI460004311159502);

重启Asterisk，在CLI中输入sip reload和dialplan reload。

在CLI中输入 sip show peers 可以看到所在线用户的ip和端口
