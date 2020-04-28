---
layout: post
title: "Notes of FFMpeg Documentation"
date: 2020-02-20 00:00:00 +0800
description: "Notes of FFMpeg Documentation" # (optional)
img:  # Add image post (optional)
fig-caption: "" # Add figcaption (optional)
tags: ['FFMPEG', 'PHP']
categories: ['FFMPEG', 'PHP']
---

## FFMPEG

ffmpeg is a very fast video and audio converter that can also grab from a live audio/video source. It can also convert between arbitrary sample rates and resize video on the fly with a high quality polyphase filter.

### Detailed Description

The transcoding process in ffmpeg for each output can be described by the following diagram:

<div align="center"><div markdown='1'>
![例图]({{site.baseurl}}/assets/img/notes-of-ffmpeg-documentation.png)
</div></div>

Simple filtergraphs are those that have exactly one input and output, both of the same type. In the above diagram they can be represented by simply inserting an additional step between decoding and encoding:

<div align="center"><div markdown='1'>
![例图]({{site.baseurl}}/assets/img/notes-of-ffmpeg-documentation1.png)
</div></div>

Simple filtergraphs are configured with the per-stream -filter option (with -vf and -af aliases for video and audio respectively). A simple filtergraph for video can look for example like this:

<div align="center"><div markdown='1'>
![例图]({{site.baseurl}}/assets/img/notes-of-ffmpeg-documentation2.png)
</div></div>

Complex filtergraphs are those which cannot be described as simply a linear processing chain applied to one stream. This is the case, for example, when the graph has more than one input and/or output, or when output stream type is different from input. They can be represented with the following diagram:

<div align="center"><div markdown='1'>
![例图]({{site.baseurl}}/assets/img/notes-of-ffmpeg-documentation3.png)
</div></div>

Stream copy is a mode selected by supplying the copy parameter to the -codec option. It makes ffmpeg omit the decoding and encoding step for the specified stream, so it does only demuxing and muxing. It is useful for changing the container format or modifying container-level metadata. The diagram above will, in this case, simplify to this:

<div align="center"><div markdown='1'>
![例图]({{site.baseurl}}/assets/img/notes-of-ffmpeg-documentation4.png)
</div></div>

## FFPROBE

ffprobe gathers information from multimedia streams and prints it in human- and machine-readable fashion.

## CODEC

Decoders are configured elements in FFmpeg which allow the decoding of multimedia streams.

Encoders are configured elements in FFmpeg which allow the encoding of multimedia streams.

## FFMPEG FORMATS

Demuxers are configured elements in FFmpeg that can read the multimedia streams from a particular type of file.

Muxers are configured elements in FFmpeg which allow writing multimedia streams to a particular type of file.