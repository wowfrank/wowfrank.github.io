---
layout: post
title: 「视频直播技术详解」系列之四：编码和封装
date: 2019-10-03 121:02:20 +0300
description: 「视频直播技术详解」系列之四：编码和封装 # Add post description (optional)
img: live-streaming-code-encapsulation.jpg # Add image post (optional)
fig-caption: none # Add figcaption (optional)
tags: [视频直播技术详解, 编码和封装, 视频直播]
categories: [blog，live-streaming]
excerpt_separator: <!--more-->
---

## **「视频直播技术详解」系列之四：编码和封装**

关于直播的技术文章不少，成体系的不多。我们将用七篇文章，更系统化地介绍当下大热的视频直播各环节的关键技术<!--more-->，帮助视频直播创业者们更全面、深入地了解视频直播技术，更好地技术选型。

视频编码是视频直播技术系列文章的第三篇，是本系列一个非常重要的部分，是移动开发必修的基础课程，本篇文章从理论到实践一网打尽主流编码器。

如果把整个流媒体比喻成一个物流系统，那么编解码就是其中配货和装货的过程，这个过程非常重要，它的速度和压缩比对物流系统的意义非常大，影响物流系统的整体速度和成本。同样，对流媒体传输来说，编码也非常重要，它的编码性能、编码速度和编码压缩比会直接影响整个流媒体传输的用户体验和传输成本。

本系列文章大纲如下，想复习之前文章的直接点击直达链接：

[（一）开篇](https://wowfrank.github.io/live-streaming-opening/)

[（二）采集](https://wowfrank.github.io/live-streaming-collection/)

[（三）处理](https://wowfrank.github.io/live-streaming-proceed/)

（四）编码和封装

[（五）推流和传输](https://wowfrank.github.io/live-streaming-push-and-transport/)

[（六）延迟优化](https://wowfrank.github.io/live-streaming-optimize-latency/)

[（七）现代播放器原理](https://wowfrank.github.io/live-streaming-theory-of-modern-player/)

[（八）SDK性能测试模型](https://wowfrank.github.io/live-streaming-model-of-sdk-testing/)

### **视频编码的意义**

* 原始视频数据存储空间大，一个1080P的7s视频需要817MB
* 原始视频数据传输占用带宽大，10Mbps的带宽传输上述7s视频需要11分钟

而经过H.264编码压缩之后，视频大小只有708k,10Mbps的带宽仅仅需要500ms，可以满足实时传输的需求，所以从视频采集传感器采集来的原始视频势必要经过视频编码。

### **基本原理**

那为什么巨大的原始视频可以编码成很小的视频呢？这其中的技术是什么呢？
核心思想就是去除冗余信息：

* 空间冗余：图像相邻像素之间有较强的相关性
* 时间冗余：视频序列的相邻图像之间内容相似
* 编码冗余：不同像素值出现的概率不同
* 视觉冗余：人的视觉系统对某些细节不敏感
* 知识冗余：规律性的结构可由先验知识和背景知识得到

视频本质上讲是一系列图片连续快速的播放，最简单的压缩方式就是对每一帧图片进行压缩，例如比较古老的MJPEG编码就是这种编码方式，这种编码方式只有帧内编码，利用空间上的取样预测来编码。形象的比喻就是把每帧都作为一张图片，采用JPEG的编码格式对图片进行压缩，这种编码只考虑了一张图片内的冗余信息压缩，如图1，绿色的部分就是当前待编码的区域，灰色就是尚未编码的区域，绿色区域可以根据已经编码的部分进行预测（绿色的左边，下边，左下等）。

<div align="center"><div markdown='1'>
![例图]({{site.baseurl}}/assets/img/live-streaming-code-encapsulation-1.jpeg)

图1
</div></div>

但是帧和帧之间因为时间的相关性，后续开发出了一些比较高级的编码器可以采用帧间编码，简单点说就是通过搜索算法选定了帧上的某些区域，然后通过计算当前帧和前后参考帧的向量差进行编码的一种形式，通过下面两个图2连续帧我们可以看到，滑雪的同学是向前位移的，但实际上是雪景在向后位移，P帧通过参考帧（I或其他P帧）就可以进行编码了，编码之后的大小非常小，压缩比非常高。

<div align="center"><div markdown='1'>
![例图]({{site.baseurl}}/assets/img/live-streaming-code-encapsulation-2.jpeg)

图2
</div></div>

可能有同学对这两张图片怎么来的感兴趣，这里用了FFmpeg的两行命令来实现，具体FFmpeg的更多内容请看后续章节：

* 第一行生成带有移动矢量的视频
* 第二行把每一帧都输出成图片

```bash
ffmpeg  -flags2 +export_mvs -i tutu.mp4 -vf codecview=mv=pf+bf+bb tutudebug2.mp4
ffmpeg -i tutudebug2.mp4 'tutunormal-%03d.bmp'
```

除了空间冗余和时间冗余的压缩，主要还有编码压缩和视觉压缩，下面是一个编码器主要的流程图：

<div align="center"><div markdown='1'>
![帧内编码图]({{site.baseurl}}/assets/img/live-streaming-code-encapsulation-3.jpeg)

图3：帧内编码图
</div></div>

<div align="center"><div markdown='1'>
![帧间编码图]({{site.baseurl}}/assets/img/live-streaming-code-encapsulation-4.jpeg)

图4：帧间编码图
</div></div>

图3、图4两个流程，图3是帧内编码，图4是帧间编码，从图上看到的主要区别就是第一步不相同，其实这两个流程也是结合在一起的，我们通常说的I帧和P帧就是分别采用了帧内编码和帧间编码。

### **编码器的选择**

前面梳理了一下编码器的原理和基本流程，编码器经历了数十年的发展，已经从开始的只支持帧内编码演进到现如今的H.265和VP9为代表的新一代编码器，就目前一些常见的编码器进行分析，带大家探索一下编码器的世界。

#### **1) H.264**

#### **简介**

H.264/AVC项目意图创建一种视频标准。与旧标准相比，它能够在更低带宽下提供优质视频（换言之，只有MPEG-2，H.263或MPEG-4第2部分的一半带宽或更少），也不增加太多设计复杂度使得无法实现或实现成本过高。另一目的是提供足够的灵活性以在各种应用、网络及系统中使用，包括高、低带宽，高、低视频分辨率，广播，DVD存储，RTP/IP网络，以及ITU-T多媒体电话系统。

H.264/AVC包含了一系列新的特征，使得它比起以前的编解码器不但能够更有效的进行编码，还能在各种网络环境下的应用中使用。这样的技术基础让H.264成为包括YouTube在内的在线视频公司采用它作为主要的编解码器，但是使用它并不是一件很轻松的事情，理论上讲使用H.264需要交纳不菲的专利费用。

#### **专利许可**

和MPEG-2第一部分、第二部分，MPEG-4第二部分一样，使用H.264/AVC的产品制造商和服务提供商需要向他们的产品所使用的专利的持有者支付专利许可费用。这些专利许可的主要来源是一家称为MPEG-LA LLC的私有组织，该组织和MPEG标准化组织没有任何关系，但是该组织也管理著MPEG-2第一部分系统、第二部分视频、MPEG-4第二部分视频和其它一些技术的专利许可。

其他的专利许可则需要向另一家称为VIA Licensing的私有组织申请，这家公司另外也管理偏向音频压缩的标准如MPEG-2 AAC及MPEG-4 Audio的专利许可。

#### **H.264 的开源实现**

* openh264
* x264

[openh264](https://link.zhihu.com/?target=https%3A//github.com/cisco/openh264){:target="_blank"}是思科实现的开源H.264编码，虽然H.264需要交纳不菲的专利费用，但是专利费有一个年度上限，思科把OpenH264实现的年度专利费交满后，OpenH264事实上就可以免费自由的使用了。

[x264](https://link.zhihu.com/?target=http%3A//www.videolan.org/developers/x264.html){:target="_blank"}是一个采用GPL授权的视频编码自由软件。x264的主要功能在于进行H.264/MPEG-4 AVC的视频编码，而不是作为解码器（decoder）之用。

除去费用问题比较来看：

* openh264 CPU 的占用相对 x264低很多
* openh264 只支持 baseline profile，x264 支持更多 profile

#### **2) HEVC/H.265**

#### **简介**

高效率视频编码（High Efficiency Video Coding，简称HEVC）是一种视频压缩标准，被视为是ITU-T H.264/MPEG-4 AVC标准的继任者。2004年开始由ISO/IEC Moving Picture Experts Group（MPEG）和ITU-T Video Coding Experts Group（VCEG）作为ISO/IEC 23008-2 MPEG-H Part 2或称作ITU-T H.265开始制定。第一版的HEVC/H.265视频压缩标准在2013年4月13日被接受为国际电信联盟（ITU-T）的正式标准。HEVC被认为不仅提升视频质量，同时也能达到H.264/MPEG-4 AVC两倍之压缩率（等同于同样画面质量下比特率减少了50%），可支持4K分辨率甚至到超高清电视（UHDTV），最高分辨率可达到 8192×4320（8K分辨率）。

#### **H.265 的开源实现**

* libde265
* x265

[libde265](https://link.zhihu.com/?target=https%3A//github.com/strukturag/libde265){:target="_blank"}HEVC由struktur公司以开源许可证GNU LesserGeneral Public License (LGPL)提供，观众可以较慢的网速下欣赏到最高品质的影像。跟以前基于H.264标准的解码器相比，libde265 HEVC解码器可以将您的全高清内容带给多达两倍的受众，或者，减少50%流媒体播放所需要的带宽。高清或者4K/8K超高清流媒体播放，低延迟/低带宽视频会议，以及完整的移动设备覆盖。具有「拥塞感知」视频编码的稳定性，十分适合应用在3/4G和LTE网络。

#### **专利许可**

HEVC Advance要求所有包括苹果、YouTube、Netflix、Facebook、亚马逊等使用H.265技术的内容制造商上缴内容收入的0.5%作为技术使用费，而整个流媒体市场每年达到约1000亿美元的规模，且不断增长中，征收0.5%绝对是一笔庞大的费用。而且他们还没有放过设备制造商，其中电视厂商需要支付每台1.5美元、移动设备厂商每台0.8美元的专利费。他们甚至没有放过蓝光设备播放器、游戏机、录像机这样的厂商，这些厂商必须支付每台1.1美元的费用。最无法令人接受的是，HEVC Advance的专利使用权追溯到了厂商的「」”，意思是之前已经发售的产品依然要追缴费用。

[x265](https://link.zhihu.com/?target=https%3A//github.com/videolan/x265){:target="_blank"}是由MulticoreWare开发，并开源。采用GPL协议，但是资助这个项目的几个公司组成了联盟可以在非GPL协议下使用这个软件。

#### **3) VP8**

#### **简介**

VP8是一个开放的视频压缩格式，最早由On2 Technologies开发，随后由Google发布。同时Google也发布了VP8编码的实做库：libvpx，以BSD授权条款的方式发行，随后也附加了专利使用权。而在经过一些争论之后，最终VP8的授权确认为一个开放源代码授权。

目前支持VP8的网页浏览器有Opera、Firefox和Chrome。

#### **专利许可**

2013年三月，Google与MPEG LA及11个专利持有者达成协议，让Google获取VP8以及其之前的VPx等编码所可能侵犯的专利授权，同时Google也可以无偿再次授权相关专利给VP8的用户，此协议同时适用于下一代VPx编码。至此MPEG LA放弃成立VP8专利集中授权联盟，VP8的用户将可确定无偿使用此编码而无须担心可能的专利侵权授权金的问题。

#### **VP8 的开源实现**

* libvpx

[libvpx](https://link.zhihu.com/?target=https%3A//github.com/webmproject/libvpx){:target="_blank"}是VP8的唯一开源实现，由On2 Technologies开发，Google收购后将其开放源码，License非常宽松可以自由使用。

#### **4) VP9**

#### **简介**

VP9的开发从2011年第三季开始，目标是在同画质下，比VP8编码减少50%的文件大小，另一个目标则是要在编码效率上超越HEVC编码。

2012年12月13日，Chromium浏览器加入了VP9编码的支持。Chrome浏览器则是在2013年2月21日开始支持 VP9 编码的视频播放。

Google宣布会在2013年6月17日完成VP9编码的制定工作，届时Chrome浏览器将会把VP9编码默认引导。2014年3月18日，Mozilla在Firefox浏览器中加入了VP9的支持。

2015年4月3日，谷歌发布了libvpx1.4.0增加了对10位和12位的比特深度支持、4:2:2 和 4:4:4 色度抽样，并VP9多核心编/解码。

#### **专利许可**

VP9是一个开放格式、无权利金的视频编码格式。

#### **VP9的开源实现**

* libvpx

libvpx是VP9的唯一开源实现，由Google开发维护，里面有部分代码是VP8和VP9公用的，其余分别是VP8和VP9的编解码实现。VP9和H.264和HEVC比较CodecHEVCx264vp9HEVC-42.2%32.6%x26475.8%18.5%vp948.3%-14.6%CodecHEVC vs. VP9(in %)VP9 vs. x264 (in %)Total Average61239399

引用Comparative Assessment of H.265/MPEG-HEVC, VP9, and
H.264/MPEG-AVC Encoders for Low-Delay Video Applications这篇比较新的论文对，低延迟视频进行编码的测试结果。

#### **HEVC和H.264在不同分辨率下的比较**

跟H.264/MPEG-4相比，HEVC的平均比特率减低值为：分辨率480P720P1080P4K UHDHEVC52%56%62%64%可见码率下降了60%以上。

* HEVC(H.265)对VP9和H.264在码率节省上有较大的优势，在相同PSNR下分别节省了 48.3% 和 75.8%。
* H.264 在编码时间上有巨大优势，对比 VP9 和 HEVC(H.265)，HEVC是VP9的6倍，VP9是H.264的将近40倍

#### **5) FFmpeg**

谈到视频编码相关内容就不得不提一个伟大的软件包 -- FFmpeg。

FFmpeg是一个自由软件，可以运行音频和视频多种格式的录影、转换、流功能，包含了libavcodec——这是一个用于多个项目中音频和视频的解码器库，以及libavformat——一个音频与视频格式转换库。

FFmpeg这个单词中的FF指的是Fast Forward。有些新手写信给FFmpeg的项目负责人，询问FF是不是代表Fast Free或者Fast Fourier等意思，FFmpeg的项目负责人回信说：「Just for the record, the original meaning of FF in FFmpeg is Fast Forward...」

这个项目最初是由Fabrice Bellard发起的，而现在是由Michael Niedermayer在进行维护。许多FFmpeg的开发者同时也是MPlayer项目的成员，FFmpeg在MPlayer项目中是被设计为服务器版本进行开发。

FFmpeg下载地址是:[FFmpeg Download](https://link.zhihu.com/?target=https%3A//ffmpeg.org/download.html){:target="_blank"}

* 可以浏览器输入下载，目前支持Linux, Mac OS, Windows三个主流的平台，也可以自己编译到Android或者iOS平台。
* 如果是Mac OS，可以通过brew安装brew install ffmpeg --with-libvpx --with-libvorbis --with-ffplay

我们可以用FFmpeg来做哪些有用有好玩的事情呢？通过一系列小实验来带大家领略FFmpeg的神奇和强大。

#### **FFmpeg录屏**

通过一个小例子看一下怎么在Mac OS下面使用FFmpeg进行录屏:

输入：

```bash
ffmpeg -f avfoundation -list_devices true -i ""
```

输出：

```bash
[AVFoundation input device @ 0x7fbec0c10940] AVFoundation video devices:
[AVFoundation input device @ 0x7fbec0c10940] [0] FaceTime HD Camera
[AVFoundation input device @ 0x7fbec0c10940] [1] Capture screen 0
[AVFoundation input device @ 0x7fbec0c10940] [2] Capture screen 1
[AVFoundation input device @ 0x7fbec0c10940] AVFoundation audio devices:
[AVFoundation input device @ 0x7fbec0c10940] [0] Built-in Microphone
```

给出了当前设备支持的所有输入设备的列表和编号，我本地有两块显示器，所以1和2都是我屏幕，可以选择一块进行录屏。

查看当前的H.264编解码器：

输入：

```bash
ffmpeg -codecs | grep 264
```

输出：

```bash
DEV.LS h264 H.264 / AVC / MPEG-4 AVC / MPEG-4 part 10 (decoders: h264 h264_vda ) (encoders: libx264 libx264rgb)
```

查看当前的VP8编解码器：

输入：

```bash
ffmpeg -codecs | grep vp8
```

输出：

```bash
  DEV.L. vp8 On2 VP8 (decoders: vp8 libvpx ) (encoders: libvpx)
```

可以选择用vp8或者h264做编码器

```bash
ffmpeg -r 30 -f avfoundation -i 1 -vcodec vp8 -quality realtime screen2.webm
# -quality realtime 用来优化编码器，如果不加在我的 Air 上帧率只能达到 2
```

or

```bash
ffmpeg -r 30 -f avfoundation -i 1 -vcodec h264 screen.mp4
```

然后用ffplay播放就可以了

```bash
ffplay screen.mp4
```

or

```bash
ffplay screen2.webp
```

#### **FFmpeg视频转换成gif**

有一个特别有用的需求，在网上发现了一个特别有趣的视频想把它转换成一个动态表情，作为一个IT从业者，我第一个想到的不是下载一个转码器，也不是去找一个在线转换网站，直接利用手边的工具FFmpeg，瞬间就完成了转码：

```bash
ffmpeg -ss 10 -t 10  -i tutu.mp4  -s 80x60  tutu.gif
## -ss 指从 10s 开始转码,-t 指转换 10s 的视频 -s
```

#### **FFmpeg 录制屏幕并直播**

可以继续扩展例子1，直播当前屏幕的内容，向大家介绍一下怎么通过几行命令搭建一个测试用的直播服务：

Step 1：首先安装docker：

访问[Docker Download](https://link.zhihu.com/?target=https%3A//www.docker.com/products/docker){:target="_blank"}，按操作系统下载安装。

Step 2：下载nginx-rtmp镜像：

```bash
docker pull chakkritte/docker-nginx-rtmp
```

Step 3：创建nginx html路径，启动docker-nginx-rtmp

```bash
mkdir ~/rtmp
docker run -d -p 80:80 -p 1935:1935 -v ~/rtmp:/usr/local/nginx/html chakkritte/docker-nginx-rtmp
```

Step 4：推送屏幕录制到nignx-rtmp

```bash
ffmpeg -y -loglevel warning -f avfoundation -i 2 -r 30 -s 480x320 -threads 2 -vcodec libx264  -f flv rtmp://127.0.0.1/live/test
```

Step 5：用ffplay播放

```bash
ffplay rtmp://127.0.0.1/live/test
```

总结一下，FFmpeg是个优秀的工具，可以通过它完成很多日常的工作和实验，但是距离提供真正可用的流媒体服务、直播服务还有非常多的工作要做，这方面可以参考七牛云发布的 七牛直播云服务 。

### **封装**

介绍完了视频编码后，再来介绍一些封装。沿用前面的比喻，封装可以理解为采用哪种货车去运输，也就是媒体的容器。

所谓容器，就是把编码器生成的多媒体内容（视频，音频，字幕，章节信息等）混合封装在一起的标准。容器使得不同多媒体内容同步播放变得很简单，而容器的另一个作用就是为多媒体内容提供索引，也就是说如果没有容器存在的话一部影片你只能从一开始看到最后，不能拖动进度条（当然这种情况下有的播放器会话比较长的时间临时创建索引），而且如果你不自己去手动另外载入音频就没有声音，下面介绍几种常见的封装格式和优缺点：

* AVI 格式（后缀为 .AVI）: 它的英文全称为 Audio Video Interleaved ，即音频视频交错格式。它于 1992 年被 Microsoft 公司推出。这种视频格式的优点是图像质量好。由于无损AVI可以保存alpha通道，经常被我们使用。缺点太多，体积过于庞大，而且更加糟糕的是压缩标准不统一，最普遍的现象就是高版本Windows媒体播放器播放不了采用早期编码编辑的AVI格式视频，而低版本 Windows 媒体播放器又播放不了采用最新编码编辑的AVI格式视频，所以我们在进行一些AVI格式的视频播放时常会出现由于视频编码问题而造成的视频不能播放或即使能够播放，但存在不能调节播放进度和播放时只有声音没有图像等一些莫名其妙的问题。

* DV-AVI 格式（后缀为 .AVI）: DV的英文全称是Digital Video Format，是由索尼、松下、JVC 等多家厂商联合提出的一种家用数字视频格式。数字摄像机就是使用这种格式记录视频数据的。它可以通过电脑的IEEE 1394端口传输视频数据到电脑，也可以将电脑中编辑好的的视频数据回录到数码摄像机中。这种视频格式的文件扩展名也是 avi。电视台采用录像带记录模拟信号，通过 EDIUS 由IEEE 1394端口采集卡从录像带中采集出来的视频就是这种格式。

* QuickTime File Format 格式（后缀为 .MOV）:美国Apple公司开发的一种视频格式，默认的播放器是苹果的QuickTime。具有较高的压缩比率和较完美的视频清晰度等特点，并可以保存alpha通道。

* MPEG 格式（文件后缀可以是 .MPG .MPEG .MPE .DAT .VOB .ASF .3GP .MP4等) : 它的英文全称为 Moving Picture Experts Group，即运动图像专家组格式，该专家组建于1988年，专门负责为CDMPEG文件格式是运动图像压缩算法的国际标准。MPEG格式目前有三个压缩标准，分别是MPEG－1、MPEG－2、和MPEG－4。MPEG－1、MPEG－2目前已经使用较少，着重介绍MPEG－4，其制定于1998年，MPEG－4是为了播放流式媒体的高质量视频而专门设计的，以求使用最少的数据获得最佳的图像质量。目前MPEG-4最有吸引力的地方在于它能够保存接近于DVD画质的小体积视频文件。

* WMV 格式（后缀为.WMV .ASF）: 它的英文全称为Windows Media Video也是微软推出的一种采用独立编码方式并且可以直接在网上实时观看视频节目的文件压缩格式。WMV格式的主要优点包括：本地或网络回放,丰富的流间关系以及扩展性等。WMV 格式需要在网站上播放，需要安装Windows Media Player（简称 WMP）很不方便，现在已经几乎没有网站采用了。

* Real Video 格式（后缀为 .RM .RMVB）:Real Networks公司所制定的音频视频压缩规范称为Real Media。用户可以使用RealPlayer根据不同的网络传输速率制定出不同的压缩比率，从而实现在低速率的网络上进行影像数据实时传送和播放。RMVB 格式：这是一种由RM视频格式升级延伸出的新视频格式，当然性能上有很大的提升。RMVB视频也是有着较明显的优势，一部大小为700MB左右的 DVD 影片，如果将其转录成同样品质的 RMVB 格式，其个头最多也就 400MB左右。大家可能注意到了，以前在网络上下载电影和视频的时候，经常接触到RMVB格式，但是随着时代的发展这种格式被越来越多的更优秀的格式替代，著名的人人影视字幕组在2013年已经宣布不再压制RMVB格式视频。

* Flash Video格式（后缀为 .FLV）:由Adobe Flash延伸出来的的一种流行网络视频封装格式。随着视频网站的丰富，这个格式已经非常普及。

* Matroska 格式（后缀为 .MKV）:是一种新的多媒体封装格式，这个封装格式可把多种不同编码的视频及16条或以上不同格式的音频和语言不同的字幕封装到一个Matroska Media档内。它也是其中一种开放源代码的多媒体封装格式。Matroska同时还可以提供非常好的交互功能，而且比MPEG的方便、强大。

* MPEG2-TS 格式 (后缀为 .ts)（Transport Stream“传输流”；又称MTS、TS）是一种传输和存储包含音效、视频与通信协议各种数据的标准格式，用于数字电视广播系统，如DVB、ATSC、IPTV等等。MPEG2-TS定义于MPEG-2第一部分，系统（即原来之ISO/IEC标准13818-1或ITU-TRec. H.222.0）。Media Player Classic、VLC多媒体播放器等软件可以直接播放MPEG-TS文件。

目前，我们在流媒体传输，尤其是直播中主要采用的就是FLV和MPEG2-TS格式，分别用于RTMP/HTTP-FLV和HLS协议。

##### 此系列文章均转载自**[牛七云](https://www.qiniu.com/){:target="_blank"}**！