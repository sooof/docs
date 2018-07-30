---
name: GSM900配置
sort: 1
---

# GSM900配置
  GSM900配置需要以下步骤：
* 刷写子板

```
/usr/local/src/gnuradio-3.3.0/usrp/host/apps/burn-db-eeprom -t rfx900_mimo_b -A –f
```
* 配置OpenBTS

```
cd /usr/local/src/openbts-2.6.0Mamou/app
打开文件Openbts.conf,修改以下参数：
# Valid band values are 850, 900, 1800, 1900.
GSM.Band 900
# Valid ARFCN range depends on the band.
GSM.ARFCN 73
```

* GSM900:        
 基站收： f1(n) = 890.2+(n-1)*0.2(MHz)
 基站发： f2(n) = f1(n)+45
