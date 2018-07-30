---
name: 软交换配置
sort: 2
---

# 软交换配置

## 配置Asterisk：

* 将OpenBTS中AsteriskConfigure/文件夹下的sip.conf和extensions.conf的内容复制到/etc/asterisk/下sip.conf和extensions.conf。若使用自动注册用户程序，则不用对sip.conf和extensions.conf文件进行编辑。

* 若使用手动注册，测、、则需修改sip.conf和extensions.conf文件，过程如下：

   * 得到SIM卡的IMSI号
   通过OpenBTS自身得到IMSI号，当手机试图接入OpenBTS时，会向OpenBTS发送IMSI号，可通过OpenBTS的CLI显示的信息中读出，如下所示：


   ```
   RadioResource.cpp:152: AccessGrantResponder RA=0x15 when=0:1192710 age=25 TOA=0.0000 ChannelDescription=(typeAndOffset=SDCCH/4-0 TN=0 TSC=0 ARFCN=975) RequestReference=(RA=21 T1'=3 T2=12 T3=24) TimingAdvance=0 MobilityManagement.cpp:119: LocationUpdatingController MM Location Updating Request  LAI=(MCC=901 MNC=55 LAC=0x29b) MobileIdentity=(TMSI=0x49ffcddd)MobilityManagement.cpp:172: LocationUpdatingController registration FAIL: IMSI=234100223456161
   ```



   * 把上一步得到的IMSI号写入sip.conf中，示例如下：

   ```
   [IMSI310074311159502] ; 460004311159502为IMSI号
   callerid=IMSI460004311159502 <2102>;<>内为该手机的号码
   canreinvite=no
   type=friend
   allow=gsm
   context=sip-external
   host=dynamic
   ```

   * 在extensions.conf中[ sip-local ]中加入如下语句：

   ```
   ; This is a simple mapping between extensions and IMSIs.
   exten => 2102,1,Macro(dialSIP,IMSI310074311159502);
   ```
   重启Asterisk，在CLI中输入sip reload和dialplan reload。在CLI中输入 sip show peers 可以看到所在线用户的ip和端口

### 本版本所使用的asterisk配置

```
root@9ecb8ec69c3b:/# vim /etc/asterisk/extensions.conf

[macro-dialSIP]
exten => s,1,Dial(SIP/${ARG1})
exten => s,2,Goto(s-${DIALSTATUS},1)
exten => s-CANCEL,1,Hangup
exten => s-NOANSWER,1,Hangup
exten => s-BUSY,1,Busy(30)
exten => s-CONGESTION,1,Congestion(30)
exten => s-CHANUNAVAIL,1,playback(ss-noservice)
exten => s-CANCEL,1,Hangup

[from-trunk]
; route incoming calls from the PSTN
exten => s,1,Answer
;exten => 17074700739,1,Dial(SIP/wiredPhone)            ; "hotline"
;exten => 17074700741,1,Dial(SIP/IMSI310410186585295)   ; 8890
;exten => 17074700742,1,Dial(SIP/zoiper)
;exten => 17074700743,1,Dial(SIP/IMSI310410186585294)   ; 5110
;exten => 17074700746,1,Dial(SIP/IMSI234100223456161)   ; Treo
;exten => 31208080896,1,Dial(SIP/IMSI310410186585289)   ; 3310

;exten => 2100,1,Dial(SIP/2100)
;exten => 2110,1,Dial(SIP/2110)
;exten => 2101,1,Dial(SIP/2101)

[sip-external]
; check for local extensions first
include => sip-local
[sip-local]
exten => _X,1,Answer()
exten => _X,n,Echo()
exten => _X,n,Hangup()
exten => 4000,1,Macro(dialSIP,4000)
exten => 4001,1,Macro(dialSIP,4001)
exten => 4002,1,Macro(dialSIP,4002)
exten => 4003,1,Macro(dialSIP,4003)
exten => 4004,1,Macro(dialSIP,4004)
exten => 4005,1,Macro(dialSIP,4005)
;exten => 2102,1,Macro(dialSIP,IMSI208123456789012)
;exten => 2103,1,Macro(dialSIP,IMSI208555555555555)
exten => 2101,1,Macro(dialSIP,IMSI310077029239681)
exten => 2102,1,Macro(dialSIP,IMSI310071773117280)
exten => 2103,1,Macro(dialSIP,IMSI310079211903523)
exten => 2104,1,Macro(dialSIP,IMSI310073299225619)
exten => 2105,1,Macro(dialSIP,IMSI310077029239684)
exten => 2106,1,Macro(dialSIP,IMSI310077029239685)
exten => 2107,1,Macro(dialSIP,IMSI310077029239687)
exten => 2108,1,Macro(dialSIP,IMSI310077029239688)
exten => 2109,1,Macro(dialSIP,IMSI310077029239689)
exten => 2110,1,Macro(dialSIP,IMSI310077029239690)
exten => 2111,1,Macro(dialSIP,IMSI310077029239691)
exten => 2112,1,Macro(dialSIP,IMSI310077029239692)
exten => 2113,1,Macro(dialSIP,IMSI310077029239693)
exten => 2114,1,Macro(dialSIP,IMSI310077029239694)
exten => 2115,1,Macro(dialSIP,IMSI310077029239695)
```
