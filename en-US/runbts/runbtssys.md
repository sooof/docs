---
name: 登陆OpenBts
sort: 2
---

# 登陆OpenBts映像

* 运行以下命令登陆Docker openbts3.09_52。

  ```
  #docker run -t -i --device=`cat find_usb_dev.txt`  openbts3.09_52:0201  /bin/bash
  ```

* 登陆上openbts3.09_52

  运行smqueue：
  
  ```
  root@9a59ca6ebc6a#cd /usr/local/src/openbts-2.6.0Mamou/smqueue/
  root@9a59ca6ebc6a/usr/local/src/openbts-2.6.0Mamou/smqueue# ./smqueue
  ```

   运行asterisk 和 OpenBTS：

   ```
   root@9a59ca6ebc6a#cd /usr/local/src/openbts-2.6.0Mamou/app
   root@9a59ca6ebc6a:/usr/local/src/openbts-2.6.0Mamou/apps# asterisk -v &
   root@9a59ca6ebc6a:/usr/local/src/openbts-2.6.0Mamou/apps# ./OpenBTS
   ```
