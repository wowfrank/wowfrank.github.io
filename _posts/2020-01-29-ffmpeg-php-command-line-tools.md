---
layout: post
title: "FFMpeg - Invoking command-line tools from PHP scripts"
date: 2020-01-29 00:00:00 +0800
description: "FFMpeg - Invoking command-line tools from PHP scripts" # (optional)
img: ffmpeg_php.webp # Add image post (optional)
fig-caption: "FFMpeg - Invoking command-line tools from PHP scripts" # Add figcaption (optional)
tags: ['PHP','FFPMEG']
categories: ['Programming', 'PHP']
---

## Introduction

This article will try to point out all the important things you need to know when using FFmpeg from within PHP web scripts. It will also show some usage examples to make things more clear. The idea can be applied to other web scripting languages too.

## Invoking command-line tools from PHP scripts

### Choosing a model

Web pages are designed to execute quickly, so that people, browsing your web site, don't have to wait too much for the response. Because if they get bored waiting, they'll just navigate off to another, more responsive (usually your competitor's) web site. That being said, the worst thing you could do is to run a command-line tool (like ffmpeg) from your web script and wait for it to finish the processing to be able to deliver the result back to the waiting online user (the exception to this are small/fast tools whose execution times are too small to notice).

What you want to do is to separate the command-line tool's long processing from your web script execution, to make your script more responsive. So, you have at least 2 options:

1. Run the command-line tool (using ​shell_exec() for example) in the background and continue with the web script execution, periodically refreshing the status page, showing the current progress

<div align="center"><div markdown='1'>
![]({{site.baseurl}}/assets/img/ffmpeg_php_1.png)
</div></div>

2. Create a new "job" (usually a new row in a table of your database) and have some background "worker" process (cron job, shell script, batch script) that will monitor that "job list" to see if there are any new jobs that need processing

<div align="center"><div markdown='1'>
![]({{site.baseurl}}/assets/img/ffmpeg_php_2.png)
</div></div>

These two methods might seem like the same thing, but they are not. The most important difference is that the 2nd approach scales better with higher web site traffic, because it allows the "worker" process to be running on the completely separate machine. Also, there can be several "worker" machines, working together, splitting the workload, with a trivial synchronization involved.

The first method is usually the first choice for most people that want to get the job done quickly, but during the time, when their website becomes popular, then their web server becomes less responsive, due to the constant cpu starvation, caused by multiple command-line tools (ffmpeg instances) running in the background. In that moment, they usually start considering the second method.

### Running ffmpeg in the background

The following examples are intended for Linux-based operating systems. For Windows-based operating systems, please continue reading this article, but after you read it, also read ​how to run a process in the background using START and CMD. That will give you an idea how to apply the same logic to Windows-based operating systems.

Let's consider the natural way to run ffmpeg from PHP scripts:

```php
<?php
	echo "Starting ffmpeg...\n\n";
	echo shell_exec("ffmpeg -i input.avi output.avi &");
	echo "Done.\n";
?>
```

There are several issues that need to be pointed out here. The first one is that, although we specified we want ffmpeg to be executed in the background (using the ampersand operator "&"), PHP script will not continue it's execution until ffmpeg has finished its execution. This is due to the fact, mentioned in one of the notes for the ​PHP's exec() function that says:

> If a program is started with this function, in order for it to continue running in the background, the output of the program must be redirected to a file or another output stream. Failing to do so will cause PHP to hang until the execution of the program ends.

Don't be confused about the example showing ​shell_exec() call instead of ​exec(). All of the ​PHP's Program execution functions share the similar code base and limitations.

So, to work around this issue, we need to do something like this:

```php
<?php
	echo "Starting ffmpeg...\n\n";
	echo shell_exec("ffmpeg -i input.avi output.avi >/dev/null 2>/dev/null &");
	echo "Done.\n";
?>
```

The part that says ">/dev/null" will redirect the standard OUTPUT (stdout) of the ffmpeg instance to /dev/null (effectively ignoring the output) and "2>/dev/null" will redirect the standard ERROR (stderr) to /dev/null (effectively ignoring any error log messages). These two can be combined into a shorter representation: ">/dev/null 2>&1". If you like, you can ​read more about I/O Redirection.

An important note should be mentioned here. The ffmpeg command-line tool uses stderr for output of error log messages and stdout is reserved for possible use of pipes (to redirect the output media stream generated from ffmpeg to some other command line tool). That being said, if you run your ffmpeg in the background, you'll most probably want to redirect the stderr to a log file, to be able to check it later.

One more thing to take care about is the standard INPUT (stdin). Command-line ffmpeg tool is designed as an interactive utility that accepts user's input (usually from keyboard) and reports the error log on the user's current screen/terminal. When we run ffmpeg in the background, we want to tell ffmpeg that no input should be accepted (nor waited for) from the stdin. We can tell this to ffmpeg, using I/O redirection again "<\/dev\/null", so that final example, to safely run ffmpeg command-line tool in the background would be similar to this:

```php
<?php
	echo "Starting ffmpeg...\n\n";
	echo shell_exec("ffmpeg -y -i input.avi output.avi </dev/null >/dev/null 2>/var/log/ffmpeg.log &");
	echo "Done.\n";
?>
```

The ​"-y" option is used to auto-overwrite the output file (output.avi) without asking for yes/no confirmation. If you need the opposite scenario, to auto-cancel the entire process if the output file already exists, then use "-n" option instead.

## Wrapper Libraries

Some PHP libraries allow wrapping ffmpeg calls into PHP objects, and give you a nice syntax to work with if you don't like to use the command line. One of these is the actively maintained ​PHP-FFMpeg. It only requires you to ​download a recent ffmpeg and ffprobe build apart from installing the PHP components. Then you can run PHP code like this:

```php
$ffmpeg = FFMpeg\FFMpeg::create();
$video = $ffmpeg->open('video.mpg');
$video
    ->filters()
    ->resize(new FFMpeg\Coordinate\Dimension(320, 240))
    ->synchronize();
$video
    ->save(new FFMpeg\Format\Video\X264(), 'export-x264.mp4')
```

Of course you need to take care of running such a task in the background. Libraries such as ​GearmanClient facilitate this.

> Note: ​ffmpeg-php is an extension that is not developed since 2007 (and requires "ffmpeg-0.4.9_pre1 or higher"), which means that you are restricted to use a very old version of ffmpeg, without possibility to update it to the latest version. Since a lot of changes/improvements are being made, inside ffmpeg's code, every day, it makes ffmpeg-php incompatible with the latest ffmpeg.