---
layout: post
title: "Ffmpeg Command Tutorial With Examples For Video and Audio"
date: 2020-02-05 00:00:00 +0800
description: "Ffmpeg Command Tutorial With Examples For Video and Audio" # (optional)
img: ffpmeg-command-tutorial.webp # Add image post (optional)
fig-caption: "Ffmpeg Command Tutorial With Examples For Video and Audio" # Add figcaption (optional)
tags: ['PHP','FFPMEG']
categories: ['Programming', 'PHP']
---

In today’s multimedia world there are a lot of different formats for video and audio. In order to use video and audio we generally need to convert and edit operations. There are different tools for this job but the most popular and useful tool is ffmpeg. Ffmpeg is used by a lot of different free and commercial software. It provide very large feature set. In this tutorial we will look most wanted and useful features like convert, resize,… of ffmpeg . Ffmpeg is a free video editing software which works from command line.

As we know ffmpeg is provided for Windows operating systems too. So following commands will work seamlessly in Windows operating systems to if ffmpeg is downloaded and setup correctly.

Check out full ffmpeg documentation from the official website, [FFMPEG Documentation](http://ffmpeg.org/documentation.html){:raget="_blank"}

## Display Video Information

Video files have different options about their structure. These information can be display with ffmpeg.

```bash
$ ffmpeg -i jellyfish-3-mbps-hd-h264.mkv
```

Ffmeg will display following information;

- _encoder_ shows video encoder of the video file
- _creation_time_ describes the create time as year,month,day, hour,minute,second
- _Duration_ describes the length of the video file in hour:minute:second format
- _Stream_ show video stream information. A video file like mkv, mp4 may have more than one stream for different purposes. In this example we have only one stream with index 0:0 . There is information about the video stream like video format resolution frame per second

## Display Audio Information

We can also get audio information from video file or audio file.

```bash
$ ffmpeg -i test.mp3
```

## Check Supported Formats

To check list of supported formats by FFmpeg, run:

```bash
$ ffmpeg -formats
```

## Convert Mkv To Mp4

Now we can start converting files into different format. We will start converting Mkv video file into Mp4 format. In this example we will convert file named jellyfish.mkv into jellyfish.mp4

```bash
$ ffmpeg -i jellyfish.mkv jellyfish.mp4
```

In the example the first thing is printing the source file information and than the convert operation starts. During the convert operation following statistical informations are provided as real time.

- _frame_ show currently processes frame count
- _fps_ show the count of frames processed in one second
- _Lsize_ shows the destination or new file size
- _time_ shows current position of the convert process in the video length in time
- _bitrate_ shows the bit size of a second length of the video

## Convert Flash and Flv To Mp4

Flash files can be converted to Mp4 like below.

```bash
$ ffmpeg -i jellyfish.flv jellyfish.mp4
```

## Convert  Mp4 To Mp3

Mp4 files are mainly used for mobile media devices or smart phones. This type of video can be converted to the mp3 audio file with the following command.

```bash
$ ffmpeg -i jellyfish.mp4 -q:a 0  jellyfish.mp3
```

### Converting video files to audio files

To convert a video file to audio file, just specify the output format as .mp3, or .ogg, or any other audio formats.

```bash
$ ffmpeg -i input.mp4 -vn output.mp3
$ ffmpeg -i input.mp4 -vn -ar 44100 -ac 2 -ab 320 -f mp3 output.mp3
```

- _-vn_ – Indicates that we have disabled video recording in the output file.
- _-ar_ – Set the audio frequency of the output file. The common values used are  22050, 44100, 48000 Hz.
- _-ac_ – Set the number of audio channels.
- _-ab_ – Indicates the audio bitrate.
- _-f_ – Output file format. In our case, it’s mp3 format.


## Convert  Mp4 To Avi

Mp4 is a popular format as stated before. In the old time avi was the most popular advanced format.

```bash
$ ffmpeg -i jellyfish.mp4   jellyfish.avi
```

## Convert  Mp4 To Gif

Gif format is generally used to show simple, low size video to the user in the web pages without video players. Gif is a picture format that can store motions as different frames in pictures.

```bash
$ ffmpeg -i jellyfish.mp4   jellyfish.gif
```

## Convert  Avi To Mp4

We can convert avi to mp4 with the following command.

```bash
$ ffmpeg -i jellyfish.avi   jellyfish.mp4
```

## Extract Audio From Video File

We can extract audio stream from the video file and save audio as a separate file in formats like aac , mp3 , vorbis etc. We will provide -vn -ab 128 options. -ab 128 specifies the bitrate. The audio extraction will be done very little time.

```bash
$ ffmpeg -i Funny.mkv -vn -ab 128 Funny.mp3
```

## Mute Audio

As we see previous example audio file is stored as separate stream. This gives the ability to mute audio of the video file. We will use -an option to mute audio.

```bash
$ ffmpeg -i Funny.mkv -an Funny_muted.mkv
```

## Removing Video Stream From A Media File

Similarly, if you don’t want video stream, you could easily remove it from the media file using ‘vn’ flag. vn stands for no video recording. In other words, this command converts the given media file into audio file.

The following command will remove the video from the given media file.

```bash
$ ffmpeg -i input.mp4 -vn output.mp3
```

## Resize Video Resolution

Video files can be resized. Scaling down the resolution will made the video file less in size. We will use -s with the new resoution x and y sizes. In the example we will resize the video as 640x480 .

```bash
$ ffmpeg -i input.mp4 -filter:v scale=1280:720 -c:a copy output.mp4
$ ffmpeg -i Funny.mkv -s 640x480 -c:a copy  Funny_resize.mkv
```

## Set The Aspect Ratio To Video

```bash
$ ffmpeg -i input.mp4 -aspect 16:9 output.mp4
```

The commonly used aspect ratios are:

- 16:9
- 4:3
- 16:10
- 5:4
- 2:21:1
- 2:35:1
- 2:39:1

## Compressing video files

It is always an good idea to reduce the media files size to lower size to save the harddrive’s space.

```bash
$ ffmpeg -i input.mp4 -vf scale=1280:-1 -c:v libx264 -preset veryslow -crf 24 output.mp4
```

Please note that you will lose the quality if you try to reduce the video file size. You can lower that crf value to 23 or lower if 24 is too aggressive.

You could also transcode the audio down a bit and make it stereo to reduce the size by including the following options.

```bash
-ac 2 -c:a aac -strict -2 -b:a 128k
```

## Compressing Audio files

```bash
$ ffmpeg -i input.mp3 -ab 128 output.mp3
```

The list of various available audio bitrates are:

- 96kbps
- 112kbps
- 128kbps
- 160kbps
- 192kbps
- 256kbps
- 320kbps


## Add Poster Image To Audio File

As audio files provides only sound the singers albums need some poster to show while playing the song. This poster image can be embedded into audio file as metadata like below.

```bash
$ ffmpeg -i Funny.mkv -i smiley.jpg  Funny_poster.mkv
$ ffmpeg -loop 1 -i inputimage.jpg -i inputaudio.mp3 -c:v libx264 -c:a aac -strict experimental -b:a 192k -shortest output.mp4
```

## Cut Video

We can cut video from specified time as specified time range. Original file will be kept without changing. We will specify the start time with -ss option and duration with -t option. In this example we will cut video from 20 seconds to 35 seconds.

```bash
$ ffmpeg -i Funny.mkv -ss 00:00:20 -codec copy -t 15  Funny_cut.mkv
```

- _–s_ – Indicates the starting time of the video clip. In our example, starting time is the 50th second.
- _-t_ – Indicates the total time duration.

## Trim Audio

```bash
$ ffmpeg -i audio.mp3 -ss 00:01:54 -to 00:06:53 -c copy output.mp3
```

## Split Video Files Into Multiple Parts

Some websites will allow you to upload only a specific size of video. In such cases, you can split the large video files into multiple smaller parts like below.

```bash
$ ffmpeg -i input.mp4 -t 00:00:30 -c copy part1.mp4 -ss 00:00:30 -codec copy part2.mp4
```

Here, **_-t**_ 00:00:30 indicates a part that is created from the start of the video to the 30th second of video. **_-ss**_ 00:00:30 shows the starting time stamp for the next part of video. It means that the 2nd part will start from the 30th second and will continue up to the end of the original video file.

## Merge Video Files

Multiple video files can be concatenated into single video file. We will provide the video file list as a text file with -f concat and -c copy options. The video files are listed like below named videos.txt

### videos.txt

part1.mkv
part2.mkv

Now we will join togather.

```bash
$ ffmpeg -f concat -i videos.txt -c copy output.mp4
```

If you get an error something like below:

```bash
[concat @ 0x555fed174cc0] Unsafe file name '/path/to/mp4'
join.txt: Operation not permitted
```

Add “-safe 0”:

```bash
$ ffmpeg -f concat -safe 0 -i videos.txt -c copy output.mp4
```

## Crop Audio File

We have previously cut the video file. There is also option to cut audio file. We will use same options with video file but the output will be an audio file. In this example we crop audio_crop.mp3

```bash
$ ffmpeg -i Funny.mkv -ss 00:00:20  -t 15  Funny_cut.mp3
$ ffmpeg -i input.mp4 -filter:v "crop=w:h:x:y" output.mp4
```

- _input.mp4_ – source video file.
- _-filter:v_ – Indicates the video filter.
- _crop_ – Indicates crop filter.
- _w_ – Width of the rectangle that we want to crop from the source video.
- _h_ – Height of the rectangle.
- _x_ – x coordinate of the rectangle that we want to crop from the source video.
- _y_ – y coordinate of the rectangle.

Let us say you want to a video with a width of 640 pixels and a height of 480 pixels, from the position (200,150), the command would be:

```bash
$ ffmpeg -i input.mp4 -filter:v "crop=640:480:200:150" output.mp4
```

## Set Bitrate Of Audio

Bitrate of a video effects the quality of the audio more bitrate means more quality but also more audio size. We can change the audio file bitrate -ab option. In the example we will change bitrate to 128k

```bash
$ ffmpeg -i Funny.mp3 -ab 128k Funny_128k.mp3
```

## Set Framerate Of Video

The framerate specifies the count of pictures in one second. High framerate means more fluid movie but costs more CPU and disk. We can change framerate with -r option. In the example we will set framerate 15

```bash
$ ffmpeg -i Funny.mkv -r 15 Funny.mp4
```

## Set Bitrate Of Video

Bitrate of video provides color density about the frames. More bitrate means more detailed colors but more in size. We can set video bitrate with ffmpeg by using -b option. In the example we will change bitrate to 100k which means 100.000 bits

```bash
$ ffmpeg -i Funny.mkv -b 100k Funny.mp4
```

## Extract Images From Video

As you know movie rippers generally provides some picture thumbnail about movies and video files. This picture thumbnail can be created with ffmpeg. We will use -r option to specify rate and -f option for the format. In the example we will create a thumbnail with rate 1.

```bash
$ ffmpeg -i Funny.mkv -r 1 -f image image-%2d.jpeg
```

- _-r_ – Set the frame rate. I.e the number of frames to be extracted into images per second. The default value is 25.
- _-f_ – Indicates the output format i.e image format in our case.
- _image-%2d.png_ – Indicates how we want to name the extracted images. In this case, the names should start like image-01.png, image-02.png, image-03.png and so on. If you use %3d, then the name of images will start like image-001.png, image-002.png and so on.

## Add Subtitles To A Video File

```bash
$ fmpeg -i input.mp4 -i subtitle.srt -map 0 -map 1 -c copy -c:v libx264 -crf 23 -preset veryfast output.mp4
```

## Preview or test video or audio files

```bash
$ ffplay video.mp4
$ ffplay audio.mp3
# To increase the video playback speed
$ ffmpeg -i input.mp4 -vf "setpts=0.5*PTS" output.mp4
# To slow down your video, you need to use a multiplier greater than 1.
$ ffmpeg -i input.mp4 -vf "setpts=4.0*PTS" output.mp4
```

本文大量内容转载自İsmail Baydan的[《Ffmpeg Command Tutorial With Examples For Video and Audio》](https://www.poftut.com/ffmpeg-command-tutorial-examples-video-audio/){:target="_blank"}