---
title: Git-自从有了远程仓库，妈妈再也不用担心我的硬盘了
date: 2018-10-24 15:15:03
tags: Git
top:
password:
description: 将本地Git仓库与Github绑定起来
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/10.24/github.jpg
---

# 第一次使用的配置
1. 当然要注册github账号了;
2. 由于本地Git仓库与Github仓库是通过SSH加密的，所以需要创建一个SSH Key;
```c
ssh-keygen -t rsa -C "youremail@example.com"

//例如我的：
//ssh-keygen -t rsa -C "852454151@qq.com"
```
3. 打开用户主目录，找到<b>.ssh</b>文件夹，打开里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人
![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/10.24/create.jpg)
 登陆Github配置SSH Keys(位置在：右上角头像->Setting->SSH and GPG keys->New SSH Key),将id_rsa.pub内容粘贴到key中;

4. 登陆Github,创建一个新的repository
![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/10.24/resign.jpg)

5. 关联远程库
```c
//将用户名，仓库名改成你自己的
git remote add origin git@github.com:tonychenn/BlogSource.git
```

# 推送本地仓库文件到Github
(第一次推送此处可能会出错，到文章下面招解决方案)

关联后，使用命令  
``` 
git push -u origin master 
``` 
第一次推送master分支的所有内容；</br>
此后，每次本地提交后，只要有必要，就可以使用命令 
```
git push origin master
``` 
推送最新修改；

此时就能在Github上看到推送上去的readme.txt文件啦

![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/10.24/see.jpg)


# 克隆Github仓库文件到本地
本地文件夹 gitbush 执行 ```git clone https://github.com/TonyChenn/BlogSource.git```

![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/10.24/pull.jpg)

# 遇到错误
<div class="note danger no-icon"><p>failed to push some refs to 'git@github.com:.../....git</p></div>
<b>原因及解决方案：</b>
出现错误的主要原因是：github中的README.md文件不在本地代码目录中

<b>解决：</b>通过:```git pull --rebase origin master ```进行代码合并

<div align="right">TonyChenn<br>2018-10-24</div>