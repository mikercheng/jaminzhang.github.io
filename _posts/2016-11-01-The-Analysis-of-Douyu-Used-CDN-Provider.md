---
layout: post
title: 斗鱼使用的 CDN 服务商分析
description: "斗鱼使用的 CDN 服务商分析"
category: CDN
avatarimg:
tags: [Douyu, CDN, Python, Flash, RTMP]
duoshuo: true
---

# 引言
今年非常火爆的直播，作为运维人员，我们应该看到这背后是由于 CDN 技术在支撑。
那么这些网络直播平台背后有都有哪些 CDN 服务商在支撑？  
下面是一篇是公开报道，[【观察】网络直播平台背后都有哪些CDN服务商在支撑？](http://www.cww.net.cn/news/html/2016/6/29/2016629053426571.htm)。  
对市场主要的一些直播平台背后采用的 CDN 服务商进行了一个总结。  
今天我主要想看看斗鱼使用了哪些 CDN 服务商。

# 斗鱼使用的 CDN 服务商

## 根据官方及公开新闻报道获取相关信息

[2015-06-30 网宿CDN服务提升斗鱼1200万日活用户体验](https://www.douyu.com/cms/new_list/201506/30/1203.shtml)  
这是斗鱼官网去年的新闻公告。  

[2015/6/29 CDN定制化服务样板 网宿提升斗鱼游戏直播用户体验](http://www.chinanetcenter.com/Home/News/390)  
这是网宿官网去年的新闻公告。

[腾讯云 客户案例  › 视频  › 斗鱼TV](https://www.qcloud.com/customer/video/dytv.html)  
这可能是由于腾讯领投 B,C 轮投资斗鱼，所以斗鱼也使用了腾讯云的 CDN 服务。

## 通过网站实际直播视频流分析

上面我们是通过官网和公开新闻报道来获知斗鱼网站的 CDN 服务商。  
但作为技术人员，我们可以通过实际网站的直播视频流来进行一个分析确认。

访问斗鱼网站任意一个正在直播的直播间，我们可以在播放器上选择线路，默认是主线路，还有其他备用线路 2、3、5。其实这些线路对应了不同的 CDN 服务商。这个我们可以通过下面的方法获取相关信息。

### 通过斗鱼网站 API 获取信息

![douyu.com-swf_api](http://jaminzhang.github.io/images/Douyu/douyu.com-swf_api.png)  

通过以上 Python 脚本获取指定直播间信息，如下图。  
![douyu.com-swf_api_cdn](http://jaminzhang.github.io/images/Douyu/douyu.com-swf_api_cdn.png)  

从上图我们可以看到 CDN 线路对应的 CDN 厂商。  

* "主线路", cdn: "ws"
* "备用线路2", cdn: "ws2"
* "备用线路3", cdn: "dl"
* "备用线路5", cdn: "tct"

从上面的英文简写，再根据之前新闻报道，我们可以较容易猜测，ws ws2 代表网宿，tct 代表腾讯云，dl 这个不太确定，帝联？

### 通过浏览器分析直播视频流获取信息

在浏览器只可以看到备用线路 5 视频流信息，如下图。

![douyu.com-CDN-Line5-CDN-Provider](http://jaminzhang.github.io/images/Douyu/douyu.com-CDN-Line5-CDN-Provider.png)  

其他线路在浏览器中获取不到相关信息。这应该是直播视频流通过 Flash Socket 连接 CDN 服务商的 RTMP 服务器的通道。  
浏览器是获取不到 Flash Socket 的通信内容的，这时我们可以使用 Wireshark 来进行抓包来分析 RTMP 协议的直播视频流，应该可以获取 CDN 节点的一些信息。（这个等以后有时间再看下）


# Ref
[移动直播火爆背后：CDN为网络“减压”](http://www.cww.net.cn/news/html/2016/6/29/2016629053426571.htm)  
[【观察】网络直播平台背后都有哪些CDN服务商在支撑？](http://www.cww.net.cn/news/html/2016/6/29/2016629053426571.htm)  
[一些斗鱼TV Web API [Some DouyuTv API]](http://430.io/-xie-dou-yu-tv-web-api-some-douyutv-api/)  
[记一次斗鱼TV弹幕爬虫经历](http://www.jianshu.com/p/ef0225b6bb0e#)  