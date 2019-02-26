---
title: API-QQ相关
date: 2017-07-30 10:12:52
tags: 
  - API
  - Android
---

<!--more-->
1. 获取QQ头像:

```
https://q2.qlogo.cn/headimg_dl?bs=QQ号&dst_uin=QQ号&dst_uin=QQ号&;dst_uin=QQ号&spec=100&url_enc=0&referer=bu_interface&term_type=PC
```
e.g.博主本人的：

https://q2.qlogo.cn/headimg_dl?bs=852454151&dst_uin=852454151&dst_uin=852454151&;dst_uin=852454151&spec=100&url_enc=0&referer=bu_interface&term_type=PC

2. 获取QQ空间头像
 
```
https://qlogo4.store.qq.com/qzone/QQ号/QQ号/100
```
e.g.博主本人的：

https://qlogo4.store.qq.com/qzone/852454151/852454151/100


3. 第三方软件打开QQ发送消息

```java
String url = "mqqwpa://im/chat?chat_type=wpa&uin=852454151&version=1";
try { 
  startActivity(new Intent(Intent._ACTION_VIEW_, Uri.parse(url)));
}
catch(Exception e) {
  Toast.makeText(this,"本机未安装QQ",Toast._LENGTH_SHORT_).show(); 
}
```
更多API,待添加...
  