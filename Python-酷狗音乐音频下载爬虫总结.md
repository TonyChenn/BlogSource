---
title: Python-酷狗音乐爬虫总结
date: 2018-11-19 21:22:28
tags: 
    - python
    - 爬虫
top:
password:
description: 输入音乐名称，下载音乐到本地
---
![](https://ws1.sinaimg.cn/mw690/006PThdlly1fxea0fndp2j30rt0j6gxa.jpg)
<!--more-->
## 前言
博主之前用过C#写爬虫，被本学期学过Python后，冲着其：简单，干净，快捷的优点，就决定学下Python爬虫，下面记载使用Python做一个酷狗音乐下载的爬虫，和实现过程中遇到的问题，如果你也在学Python爬虫，希望这篇文章能给你带来一些帮助。


## 什么是爬虫？
1. 访问网址，抓取其html文档或者Json文档,对于某些网站具有反爬虫机制，所以需要模拟浏览器的用户代理（User Agent）的行为构造请求。
2. 筛选出第一步拿到的网页数据，筛选出需要的数据

## 获取搜索音乐列表Json
通过网页解析网页从而拿到音乐数据。我们知道在酷狗中搜索歌曲时会访问一个固定的网址+搜索内容获取数据，如：[https://www.kugou.com/yy/html/search.html#searchType=song&searchKeyWord=卡路里](https://www.kugou.com/yy/html/search.html#searchType=song&searchKeyWord=卡路里)

![](https://ws1.sinaimg.cn/large/006PThdlly1fxdfrtm2bcj31es0qz0xh.jpg)

找到网页请求的Json数据，然后解析出来：老规矩F12。嗯，找到了：

![](https://ws1.sinaimg.cn/large/006PThdlly1fxdgqxzvsoj30y20pg78b.jpg)，看下地址,

[https://songsearch.kugou.com/song_search_v2?callback=jQuery112407293972860894198_1542616202354&keyword=卡路里&page=1&pagesize=30&userid=-1&clientver=&platform=WebFilter&tag=em&filter=2&iscorrection=1&privilege_filter=0&_=1542616202356](https://songsearch.kugou.com/song_search_v2?callback=jQuery112407293972860894198_1542616202354&keyword=卡路里&page=1&pagesize=30&userid=-1&clientver=&platform=WebFilter&tag=em&filter=2&iscorrection=1&privilege_filter=0&_=1542616202356)我们就可以通过此链接获取无门需要搜索歌曲的列表。但是分析下这个文件还是没有找到播放音乐的url,额...继续找...

## 获取歌曲信息Json
点开一个音乐播放的条目，F12分析：

![](https://ws1.sinaimg.cn/mw690/006PThdlly1fxdh0ku8j1j30lg0843yu.jpg)

竟然不存在音频文件，但事实音乐已经开始播放了，并且自动播放的。那只能说明播放音乐的url依然不再这里，老办法，查找json文件：

![](https://ws1.sinaimg.cn/mw690/006PThdlly1fxdh6imwhej31f70qp7hv.jpg)

嗯，没看错，就是在这里，那么获取这个的json的url是什么呢？怎么得到呢？

看链接：[https://www.kugou.com/song/#hash=BEDD046FB30A0C443CD6F854574B065E&album_id=9175221](https://www.kugou.com/song/#hash=BEDD046FB30A0C443CD6F854574B065E&album_id=9175221) 也是一个固定的字符串与hash，album_id 组成的。那问题就变成了如何获取hash 和 album_id. 还记得之前获取的音乐列表的信息吗，当时只看懂了<b>SongName</b>,仔细对比查看：

> FileHash 对应 hash
AlbumID 对应 album_id

## Json处理
上面获取的音乐列表，音乐详细信息都不是标准的json文本，我们需要的是将其更正为标准的json
- 音乐列表json处理：

![](https://ws1.sinaimg.cn/mw690/006PThdlly1fxdnrmqfdpj30js0eut9n.jpg)
- 音乐详细信息的Json处理：

![](https://ws1.sinaimg.cn/mw690/006PThdlly1fxdoj1k9iwj30m50fbdga.jpg)

## 代码实现：
```python
import requests
import json
import re


# 将不规范的Json字符串，通过截取，返回规范的json数据
# 返回歌曲列表的Json
def get_song_list_json(unjson):
    start_index = unjson.index('[')
    end_index = unjson.rindex(']')
    unjson = unjson[start_index: end_index]

    end_index = unjson.rindex(']')
    # 此句过后，content为正常的Json文件
    str_json = unjson[0:end_index] + ']'
    return str_json


# 返回每首歌曲的信息Json
def get_music_info_json(unjson):
    start_index = unjson.index('{')
    end_index = unjson.rindex('}')
    unjson = unjson[start_index: end_index]
    start_index = unjson.index('{', 1)
    unjson = unjson[start_index: -1]+'}'
    return unjson


# 返回播放Url
def get_play_url(filehash, id=0):
    return 'https://www.kugou.com/song/#hash=' + filehash+'&album_id='+id


# 返回下载Url
def get_download_api(callback, hash, id):
    return 'https://wwwapi.kugou.com/yy/index.php?r=play/getdata&callback='+callback+'&hash='+hash

song_name = input("请输入歌曲名称：")
# 获取一条不正常的Json
url = 'https://songsearch.kugou.com/song_search_v2?callback=jQuery1124043482941576795797_1542458757544&keyword={0}&page=1&pagesize=30&userid=-1&clientver=&platform=WebFilter&tag=em&filter=2&iscorrection=1&privilege_filter=0&_=1542458757546'.format(song_name)

# 下面将不正常的json改正常
# 获取不正常Json内容
content = requests.get(url).text
# 获取Callback码
callback_end_index = content.index('(')
callback = content[0: callback_end_index]

content = get_song_list_json(content)

#解析json，返回音乐列表
music_list = json.loads(content)
imforlist = []

# 显示获取到的歌曲信息,并存放在imforlist列表中
num = 1
for item in music_list:
    imforlist.append(item)
    print(str(num)+'. ' + dict(item)['SongName'])
    num = int(num)+1

# 输入下载编号，下载对应歌曲
id = input('请输入下载编号：')
if int(id) > 0 and int(id) <= int(num):
    item = dict(imforlist[int(id)-1])
    download_api = get_download_api(callback, item['FileHash'], item['AlbumID'])
    unjson = requests.get(download_api).text
    download_url = re.findall(r'"play_url":"(.*?)"', unjson)
    download_url = str(download_url[0]).replace("\/", "/")
    print(download_url)
    with open(str(id)+'.mp3', 'wb') as fp:
        fp.write(requests.get(download_url).content)
    print('下载成功')

```

## 运行结果

![](https://ws1.sinaimg.cn/mw690/006PThdlly1fxdoorackrj30rd0a8wg7.jpg)

![](https://ws1.sinaimg.cn/mw690/006PThdlly1fxdoqrx6nej30ld05njrz.jpg)

> 仅供学习使用，用于商业用途，本人不负责！

<div align='right'>TonyChenn<br>2018.11.19</div>