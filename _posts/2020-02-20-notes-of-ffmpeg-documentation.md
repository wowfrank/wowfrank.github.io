---
layout: post
title: "Notes of FFMpeg Documentation"
date: 2020-02-20 00:00:00 +0800
description: "Notes of FFMpeg Documentation" # (optional)
img: notes-of-ffmpeg-documentation_0.png # Add image post (optional)
fig-caption: "Notes of FFMpeg Documentation" # Add figcaption (optional)
tags: ['FFMPEG']
categories: ['FFMPEG']
---

## FFMPEG

ffmpeg is a very fast video and audio converter that can also grab from a live audio/video source. It can also convert between arbitrary sample rates and resize video on the fly with a high quality polyphase filter.

### Detailed Description

The transcoding process in ffmpeg for each output can be described by the following diagram:

![例图]({{site.baseurl}}/assets/img/notes-of-ffmpeg-documentation.png)

Simple filtergraphs are those that have exactly one input and output, both of the same type. In the above diagram they can be represented by simply inserting an additional step between decoding and encoding:

![例图]({{site.baseurl}}/assets/img/notes-of-ffmpeg-documentation1.png)

Simple filtergraphs are configured with the per-stream -filter option (with -vf and -af aliases for video and audio respectively). A simple filtergraph for video can look for example like this:

![例图]({{site.baseurl}}/assets/img/notes-of-ffmpeg-documentation2.png)

Complex filtergraphs are those which cannot be described as simply a linear processing chain applied to one stream. This is the case, for example, when the graph has more than one input and/or output, or when output stream type is different from input. They can be represented with the following diagram:

![例图]({{site.baseurl}}/assets/img/notes-of-ffmpeg-documentation3.png)

Stream copy is a mode selected by supplying the copy parameter to the -codec option. It makes ffmpeg omit the decoding and encoding step for the specified stream, so it does only demuxing and muxing. It is useful for changing the container format or modifying container-level metadata. The diagram above will, in this case, simplify to this:

![例图]({{site.baseurl}}/assets/img/notes-of-ffmpeg-documentation4.png)

## FFPROBE

ffprobe gathers information from multimedia streams and prints it in human- and machine-readable fashion.

## CODEC

Decoders are configured elements in FFmpeg which allow the decoding of multimedia streams.

Encoders are configured elements in FFmpeg which allow the encoding of multimedia streams.

## FFMPEG FORMATS

Demuxers are configured elements in FFmpeg that can read the multimedia streams from a particular type of file.

Muxers are configured elements in FFmpeg which allow writing multimedia streams to a particular type of file.

---
---

FFmpeg基本组成模块FFmpeg框架的基本组成包含AVFormat、AVCodec、AVFilter、AVDevice、AVUtil等模块库

* FFmpeg的封装模块AVFormat

AVFormat中实现了目前多媒体领域中的绝大多数媒体封装格式，包括封装和解封装，如MP4、FLV、KV、TS等文件封装格式，RTMP、RTSP、MMS、HLS等网络协议封装格式。FFmpeg是否支持某种媒体封装格式，取决于编译时是否包含了该格式的封装库。根据实际需求，可进行媒体封装格式的扩展，增加自己定制的封装格式，即在AVFormat中增加自己的封装处理模块。

* FFmpeg的编解码模块AVCodec

AVCodec中实现了目前多媒体领域绝大多数常用的编解码格式，既支持编码，也支持解码。AVCodec除了支持MPEG4、AAC、MJPEG等自带的媒体编解码格式之外，还支持第三方的编解码器，如H.264（AVC）编码，需要使用x264编码器；H.265（HEVC）编码，需要使用x265编码器；MP3（mp3lame）编码，需要使用libmp3lame编码器。如果希望增加自己的编码格式，或者硬件编解码，则需要在AVCodec中增加相应的编解码模块，关于AVCode的更多相关信息以及使用信息将会在后面的章节中进行详细的介绍。

* FFmpeg的滤镜模块AVFilter

AVFilter库提供了一个通用的音频、视频、字幕等滤镜处理框架。在AVFilter中，滤镜框架可以有多个输入和多个输出。我们参考下面这个滤镜处理的例子:

![AVFilter使用样例]({{site.baseurl}}/assets/img/notes-of-ffmpeg-documentation5.png)

- 相同的Filter线性链之间用逗号分隔
- 不同的Filter线性链之间用分号分隔

* FFmpeg的视频图像转换计算模块swscale

swscale模块提供了高级别的图像转换API，例如它允许进行图像缩放和像素格式转换，常见于将图像从1080p转换成720p或者480p等的缩放，或者将图像数据从YUV420P转换成YUYV，或者YUV转RGB等图像格式转换。

* FFmpeg的音频转换计算模块swresample

swresample模块提供了高级别的音频重采样API。例如它允许操作音频采样、音频通道布局转换与布局调整。