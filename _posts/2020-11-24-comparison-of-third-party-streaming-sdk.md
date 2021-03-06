---
layout: post
title: "第三方直播SDK对比"
date: 2020-11-24 00:01:00 +0800
description: "第三方直播SDK对比" # (optional)
img: mobile-app-streaming-workflow-2280x828.png # Add image post (optional)
fig-caption: "第三方直播SDK对比" # Add figcaption (optional)
tags: ['Streaming', 'SDK']
categories: ['Streaming', 'SDK']
---

> 前言：由于现在直播很火，新加入的公司打算做直播功能，之前没接触于是先去看了下主流第三方平台的SDK，想看下哪个平台的更好一些。<!--more-->本文没什么技术含量，仅仅是将相关官网的资料整理，做了一点对比，方便看到各平台优点。

首先看过各个平台直播SDK后大致知道平台SDK分为有2种：

1. 直播：传统方式，1个主播，多个观众
2. 互动直播：与普通的单向直播相比，赋予了观众“露脸发声”的权利，因此对实时性、抗回声的要求更高；主打“连麦”、“多画面特效”等能力

## 主要功能对比

|		功能点 	| 		腾讯云 	| 		阿里云 	| 	网易云信 	| 	七牛云 	| 	金山云		| 声网 			| 	即构科技		|
| :------------- |:-------------|:-------------|:-------------|:-------------|:-------------|:-------------|:-------------|
|	文档更新时间 	| 2019-05-15	| 2019-04-03	| 2018-11-20	| 2018-05-30	| 018-12-18	| 2019-04-01	| 2019-05-15	|
|	案例			| 龙珠直播、now直播、小程序,斗鱼	| 全民直播，好未来，淘宝网	| 网易云课堂	| 熊猫直播、全民直播、龙珠直播	| 今日头条、龙珠直播	| 陌陌，花椒直播，狼人杀，斗鱼直播，B站	| 花椒直播，映客直播 |
|	直播推流	| RTMP，录屏推流	| RTMP	| RTMP	| RTMP	| RTMP | | |
| 直播播放	| RTMP、FLV 及 HLS	| RTMP、FLV及HLS	| RTMP、FLV及HLS	| RTMP-FLV、HTTP-FLV、HLS、HTTPS、mp4、mp4v	| RTMP/HTTP-FLV/HLS/HTTPS | | |
|	直播连麦	| 1对1、1对多、多对多	| N/A	| 支持4人同时语音、视频连麦互动并直播出去	| 1对1、1对多、	| 支持1对1连麦，1对多连麦处于开发中	| 1对1、1对多、多对多	| 业内首创 |
|	AI美颜特效	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持 |
|	H5页面及小程序播放	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持 |
|	支持android最低版本	| Android 4.1	| Android 4.3	| Android 4.3	| Android 4.0.3	| Android 4.0.4	| Android 4.1	| Android 4.0.3 |
|	支持IOS最低版本	| iOS 9.0	| iOS 8.0	| iOS 87.0	| iOS 8.0	| iOS 7.0	| iOS 8.0	| iOS 7.0 |
|	多主播互动	| 10人	| N/A	| 4人（需接入网易云信IM账号体系）	| 没有看到说明，翻api看到默认3人	| 从2017年6月起，金山云自研连麦不再开放给普通用户使用	| 17人	| 32人 |
|	最多观众人数	| 100万	| 	| 	| 	| 	| 	100万	| 	|


**总的来说，腾讯云直播，七牛云，金山云更偏向于娱乐性的直播，网易云信是基于他的IM系统，而阿里云偏向服务器CDN，声网更擅长多对多音视频聊天，即构科技连麦技术最强大。**

## 推流SDK其他功能比较

由于金山云有个功能列表，所以列表是按金山云来列的（以下仅供参考，实际以官方文档为准）

|		功能点 	| 		腾讯云 	| 		阿里云 	| 	网易云信 	| 	七牛云 	| 	金山云		| 声网 			| 	即构科技		|
| :------------- |:-------------|:-------------|:-------------|:-------------|:-------------|:-------------|:-------------|
| 推流地址自定义	| 支持	| N/A	| 支持	| N/A	| 支持	| 支持	| 支持 	|
| 视频软编码	| 支持	| N/A	| 支持	| 支持	| 支持	| 支持	| 支持 	|
| 视频硬编码	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持 	|
| 美颜	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持 	|
| 第三方美颜接口	| N/A	| N/A	| N/A	| 支持	| 支持	| N/A	| 支持 	|
| 水印	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持 	|
| 截图	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持 	|
| 多视频分辨率支持	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持 	|
| 横竖屏推流	| 支持	| 支持	| 	| 支持	| 支持	| 支持	| 支持 	|
| 动态横竖屏切换	| 支持	| N/A	| 	| 支持	| 支持	| 支持	| 支持 	|
| 连麦	| 支持	| N/A	| 支持	| 支持	| 支持	| 支持	| 支持 	|
| 画中画	| 支持	| N/A	| 支持	| 支持	| 支持	| 支持	| 支持 	|
| 对焦/变焦	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持 	|
| 镜像	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持 	|
| 闪光灯	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持 	|
| 耳返	| 支持	| 支持	| 	| 支持	| 支持	| 支持	| 支持 	|
| 蓝牙耳机	| 支持	| N/A	| 	| 支持	| 支持	| 支持	| 支持 	|
| 混音	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持 	|
| 混响	| 支持	| N/A	| 支持	| N/A	| 支持	| 支持	| 支持 	|
| 纯音频推流	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持 	|
| 后台音频推流	| 支持	| 支持	| 	| 支持	| 支持	| 支持	| 支持 	|
| 录屏	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持 	|
| 短视频录制	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持 	|
| 场景编码	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持 	|
| 动态帧率	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持 	|
| 变声	| 支持	| N/A	| 	| 支持	| 支持	| 支持	| 支持 	|
| 升降调	| 支持	| N/A	| 	| 支持	| 支持	| 支持	| 支持 	|
| 立体声推流	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持	| 支持 	|
| 悬浮窗	| 支持	| N/A	| 	| 	| 支持	| 	|  	|
| 降噪	| N/A	| 支持	| 	| 	| 支持	| 	| 支持 	|

## 拉流SDK其他功能比较

拉流详情我没有细看比较，基本都是播放之类的，常见播放器功能都有。

## 价格对比

### [腾讯云](https://cloud.tencent.com/)

#### 1.预付模式

流量/带宽的单位进制为1000，即：1TB = 1000GB。

|	直播流量资源包 	| 价格（元） 	| 	连麦资源包（分钟数） 	| 	价格（元） 	| 
| :------------- |:-------------|:-------------|:-------------|
|	100GB    	| 25	| 50000	| 2688（文档&工单）|
|	500GB    	| 118	| 250000	| 9688（技术人员邀您加入技术群）|
|	1TB     	| 236	| 1000000	| 28988（技术人员邀您加入技术群）|
|	5TB     	| 1086	| 3000000	| 73888（单独拉群专人支持）|
|	10TB   		| 2172   赠SDK(1年)	| 	| |
|	50TB   		| 8972   赠SDK(1年)	| 	| |
|	200TB   	| 30198  赠SDK(1年)	| 	| |
|	1PB   		| 149589 赠SDK(1年)	| 	| |

#### 1.后付费模式

|	流量阶梯 	| 价格(元/GB/天) 	|
| :------------- |:-------------|
|	0 - 500GB	|	0.26 		|
|	500GB（含）- 2TB	|	0.25 		|
|	2TB（含）- 50TB	|	0.23 		|
|	50TB（含）- 100TB	|	0.19 		|
|	≥ 100TB	|	0.16 		|

|	带宽阶梯 	| 价格(元/Mbps/天)	|
| :------------- |:-------------|
|	0 - 500Mbps	|		0.64 		|
|	500Mbps（含）- 5Gbps	|	0.62 		|
|	5Gbps（含）- 20Gbps	|	0.59 		|
|	≥ 20Gbps	|	0.58 		|

### [阿里云](https://helpcdn.aliyun.com)

#### 1.按峰值带宽计费：是以当日直播观看区域所在节点直播加速服务分别产生的带宽最高值（单位Mbps）为结算标准。 使用直播95峰值带宽计费，需提交工单申请阿里云

|	带宽阶梯 	| 价格(元/Mbps/天)	|
| :------------- |:-------------|
|	0 - 500Mbps	|		0.66 		|
|	500Mbps（含）- 5Gbps	|	0.638 		|
|	5Gbps（含）- 20Gbps	|	0.616 		|
|	≥ 20Gbps	|	0.594 		|

#### 2. 流量阶梯价格计费：流量累积到自然月底，下月自动清零重新累积。

|	流量阶梯 	| 价格(元/GB/天) 	|
| :------------- |:-------------|
|	0-10TB（含）	|	0.264 		|
|	10TB-50TB（含）	|	0.253 		|
|	50TB-100TB（含）	|	0.231 		|
|	100TB-1PB（含）	|	0.198 		|
|	大于1PB	|	0.165 		|

还有其他转码，截图，鉴黄识别，广告识别等等收费内容

### [网易云信](https://netease.im)

#### 1“按流量”计费 = 流量费 + 增值服务费；

计费标准: 1元/GB

计费规则: 按视频直播服务消耗的上下行流量之和计费

计费周期: 按日计费

收费示例:

假设当日视频直播服务消耗的上下行流量之和为1000GB，则对应的日流量费为 (1000\*1）元，即 1000元

#### 2. “按日峰值带宽”计费 = 日峰值带宽费 + 增值服务费；

计费标准:  0.6元/Mbps/日

计费规则: 当日使用直播服务产生的上下行带宽之和峰值计费（单位Mbps）

计费周期: 按日计费

收费示例:

假设当日的峰值带宽为900Mbps，则对应的日带宽计费为 (900\*0.6) 元，即 540元

#### 增值服务费：

截图标准: 0.1元/千次

实时转码标准: 0.1元 / 分钟

### [七牛云](https://www.qiniu.com/)

官网没有看到介绍

### [金山云](https://www.ksyun.com)

采用阶梯累进计费方式，价格阶梯如下：

|	流量阶梯 	| 价格(元/GB/天) 	|
| :------------- |:-------------|
|	0-10TB（含）	|	0.22 		|
|	10TB-50TB（含）	|	0.20 		|
|	50TB-100TB（含）	|	0.18 		|
|	100TB-1PB（含）	|	0.15 		|
|	大于1PB	|	0.13 		|

|	峰值带宽阶梯 	| 价格(元/Mbps/天)	|
| :------------- |:-------------|
|	0-500Mbps（含）	|		0.60 		|
|	500Mbps-5Gbps（含）	|	0.56 		|
|	大于5Gbps	|	0.52 		|

### [声网](https://docs.agora.io)

每月免费使用10000分钟，不超过完全免费；超过部分单独计算：

分辨率720P 及以下28 元 / 1000分钟，

分辨率720P 以上105 元 / 1000分钟。

### [即构科技](https://www.zego.im/)

官网没有看到介绍

## 以按流量计费做的一个横向对比（720分辨率）

**注意声网是按分钟收费，不是流量，写在这里只是做个参照**

|		阶梯 	| 	腾讯云（G）	| 	阿里云（G） 	| 	网易云信（G） | 	七牛云 	| 	金山云（G）	| 声网（分钟）	| 	即构科技		|
| :------------- |:-------------|:-------------|:-------------|:-------------|:-------------|:-------------|:-------------|
|	0 - 500GB 	| 0.26 			| 	0.264		| 1 			| 没有找到	| 0.22 			| 10000分钟以内免费 | 没有找到 |
|	500GB（含）- 2TB | 0.25 		| 	0.264		| 1 		| 			| 0.22 			| 0.28			 | 			|
|	2-10TB（含） | 0.23 			| 	0.264		| 1 		| 			| 0.22 			| 0.28			 | 			|
|	10TB-50TB（含） | 0.23 		| 	0.253		| 1 		| 			| 0.20 			| 0.28			 | 			|
|	50TB-100TB（含） | 0.19 		| 	0.231		| 1 		| 			| 0.18 			| 0.28			 | 			|
|	100TB-1PB（含） | 0.16 		| 	0.198		| 1 		| 			| 0.15 			| 0.28			 | 			|
|	大于1PB 		| 0.16 			| 	0.165		| 1 		| 			| 0.13			| 0.28			 | 			|

## PS：直播，实时音视频，互动直播，旁路直播的概念：

1.直播：（一对多，RTMP/HLS/HTTP-FLV，CDN）直播是一种非常典型的流媒体系统，通常会分为推流端（Pusher）、拉流端（或者叫播放端，Player）以及直播流媒体中心（直播源站），通常会使用CDN进行直播的分发，因此大部分情况下使用的是通用标准的协议，如RTMP，而经过CDN分发后，播放时一般可以选择RTMP、HTTP-FLV或HLS（H5支持）等方式。直播的特点是只有一个推流端，以及多个的观看端。

2.实时音视频：（双人/多人通话，UDP私有协议，低延时）实时音视频（Real-Time Communication, RTC）主要应用场景是音视频通话，技术关注点是低延时通信，因而使用基于UDP的私有协议，其延迟可低于100ms，适用于双人通话或是多人群组群话，典型的场景就是QQ电话、微信电话。 腾讯云实时音视频（TRTC）覆盖各平台，除了iOS/Android/Windows之后，还支持小程序以及 WebRTC 互通，并且支持通过云端混流的方式将画面旁路直播出去。当业务对延迟敏感，通话场景要求比较高，或是需要小程序或者 H5 场景下的双人或多人音视频通话可以选择实时音视频 TRTC。

3.互动直播：（连麦，二对多/多对多，私有协议+标准协议，DC/OC+CDN）

互动直播是在实时音视频的基础上，将实时音视频某个房间中的画面经云端混流后，通过旁路直播的方式直播出来。因此，互动直播主播与连麦者之间延迟与实时音视频一致，而主播/连麦者与普通观众之间的延时则与普通直播相同。

4.旁路直播（关键词：云端混流，转推，CDN）将主/副播实时音视频通话时的整个房间的画面复制一份到云端进行云端混流，并将混流后的画面推流给腾讯云直播系统的工作方式。 因为混流后的视频数据流和主/副播通话房间实际上并不是同一路流，而是在另外平行的一路，因而称为旁路，即不在主路。云端录制时，录制的流也是通过旁路的方式从流媒体中心引出，存到COS中。
