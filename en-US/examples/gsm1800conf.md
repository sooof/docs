---
name: GSM1800配置
sort: 1
---

# GSM1800配置
  GSM1800配置需要以下步骤：
* 刷写子板

```
/usr/local/src/gnuradio-3.3.0/usrp/host/apps/burn-db-eeprom -t rfx1800_mimo_b -A –f
```

* 配置OpenBTS

```
cd /usr/local/src/openbts-2.6.0Mamou/app
打开文件Openbts.conf,修改以下参数：
# Valid band values are 850, 900, 1800, 1900.
GSM.Band 1800
# Valid ARFCN range depends on the band.
GSM.ARFCN 585
```

* GSM1800:        
			 基站收： f1(n) = 1710+(n-511)*0.2(MHz)
			 基站发： f2(n) = f1(n)+95
