---
layout: post
title: "How to Use FFMPEG in Your Laravel Projects"
date: 2020-02-08 00:00:00 +0800
description: "How to Use FFMPEG in Your Laravel Projects" # (optional)
img: laravel-ffmpeg.jpg # Add image post (optional)
fig-caption: "How to Use FFMPEG in Your Laravel Projects" # Add figcaption (optional)
tags: ['Laravel','FFPMEG', 'PHP']
categories: ['Programming', 'PHP']
---

After some issues that got opened on GitHub I decided to write a blogpost on the Laravel FFmpeg package we created. We use this package ourselves in three different production environments. The example below is not taken from one of these projects but the goal is to give you a sense of what you could do with this package. Of course this example is just one way of doing it. Feel free to adopt if you like it or change it where needed.

I'm not digging into writing tests but if you're interested I suggest to take a look at Test Driven Laravel. It's great if you want to learn more about testing! The package itself is compatible with Laravel 5.1 and up but for this blogpost I'll use Laravel 5.4.

What we'll build is a controller that stores an uploaded video and then dispatches two jobs that will process the video. In this example I'll use three different Filesystem disks. One non-public disk to store the original uploaded video, one public disk to store a low-bitrate version of the video and another public disk to store a HLS export to do HTTP streaming. The names of these disks are videos_disk, downloadable_videos and streamable_videos. I'll not dig into the configuration of these disks, you can find it in the Laravel documentation.

## Database

First let's generate a Video model and make sure we create a controller and database migration as well:

```bash
php artisan make:model Video --migration --controller
```

Since this is an example app, the database migration is quite simple.

```php
Schema::create('videos', function (Blueprint $table) {
    $table->increments('id');
    $table->string('title');
    $table->string('original_name');
    $table->string('disk');
    $table->string('path');
    $table->datetime('converted_for_downloading_at')->nullable();
    $table->datetime('converted_for_streaming_at')->nullable();
    $table->timestamps();
});
```

Personally I don't use Eloquent's mass-assignment protection so I define the $guarded property of the Video model as an empty array and fill the $dates property with the two datetime columns.

```php
class Video extends Model
{
    protected $dates = [
        'converted_for_downloading_at',
        'converted_for_streaming_at',
    ];

    protected $guarded = [];
}
```

## HTTP layer

Before we can start working on the controller, we need a form request class to validate the user's input. I prefer to keep validation rules out of the controllers but that's just a personal preference. You could perfectly put the validation logic in your controller. Besides the form request class, let's generate two job classes so we can queue some video processing.

php artisan make:request StoreVideoRequest
php artisan make:job ConvertVideoForDownloading
php artisan make:job ConvertVideoForStreaming
Make the authorize method of the StoreVideoRequest class returns true (or implement your own authorization logic) and fill the rules method with the necessary rules. Replace the mime types with the actual types you want to support.

```php
public function rules()
{
    return [
        'title' => 'required',
        'video' => 'required|file|mimetypes:video/mp4,video/mpeg,video/x-matroska',
    ];
}
```

Alright now it's time for the controller. We'll use a store method to upload the video file, save it to the database and dispatch the jobs. The learn more about queues and jobs, please visit the Laravel documentation. In this example I'll return a JSON response containing the ID of the video but of course you can implement your own response. Don't forget to register this route in your routes file!

```php
<?php

namespace App\Http\Controllers;

use App\Http\Requests\StoreVideoRequest;
use App\Jobs\ConvertVideoForDownloading;
use App\Jobs\ConvertVideoForStreaming;
use App\Video;

class VideoController extends Controller
{
    public function store(StoreVideoRequest $request)
    {
        $video = Video::create([
            'disk'          => 'videos_disk',
            'original_name' => $request->video->getClientOriginalName(),
            'path'          => $request->video->store('videos', 'videos_disk'),
            'title'         => $request->title,
        ]);

        $this->dispatch(new ConvertVideoForDownloading($video));
        $this->dispatch(new ConvertVideoForStreaming($video));

        return response()->json([
            'id' => $video->id,
        ], 201);
    }
}
```

## FFmpeg conversions

For the first conversion I want a low-bitrate and resized version of the video. Install the FFmpeg package via composer and add the service provider and facade to your config files. Some of the opened issues on GitHub concern Facades. Please read the Laravel documentation about Facades carefully! Also take a look at the laravel-ffmpeg.php configuration file and especially the binaries settings. As you can see, the package allows you to chain all the methods but I added some comments to show you what's going on.

```php
<?php

namespace App\Jobs;

use App\Video;
use Carbon\Carbon;
use FFMpeg;
use FFMpeg\Coordinate\Dimension;
use FFMpeg\Format\Video\X264;
use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Queue\SerializesModels;

class ConvertVideoForDownloading implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;

    public $video;

    public function __construct(Video $video)
    {
        $this->video = $video;
    }

    public function handle()
    {
        // create a video format...
        $lowBitrateFormat = (new X264)->setKiloBitrate(500);

        // open the uploaded video from the right disk...
        FFMpeg::fromDisk($this->video->disk)
            ->open($this->video->path)

        // add the 'resize' filter...
            ->addFilter(function ($filters) {
                $filters->resize(new Dimension(960, 540));
            })

        // call the 'export' method...
            ->export()

        // tell the MediaExporter to which disk and in which format we want to export...
            ->toDisk('downloadable_videos')
            ->inFormat($lowBitrateFormat)

        // call the 'save' method with a filename...
            ->save($this->video->id . '.mp4');

        // update the database so we know the convertion is done!
        $this->video->update([
            'converted_for_downloading_at' => Carbon::now(),
        ]);
    }
}
```

Now let's create the second job! The beauty of HLS is that you can specify multiple bitrates. Here's a quote from Wikipedia:

> To enable a player to adapt to the bandwidth of the network, the original video is encoded in several distinct quality levels. The server serves an index, called a "master playlist", of these encodings, called "variant streams". The player can then choose between the variant streams during playback, changing back and forth seamlessly as network conditions change.

The package handles all the playlist stuff for you. The only thing you have to do is specify the different formats.

```php
<?php

namespace App\Jobs;

use App\Video;
use Carbon\Carbon;
use FFMpeg;
use FFMpeg\Format\Video\X264;
use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Queue\SerializesModels;

class ConvertVideoForStreaming implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;

    public $video;

    public function __construct(Video $video)
    {
        $this->video = $video;
    }

    public function handle()
    {
        // create some video formats...
        $lowBitrateFormat  = (new X264)->setKiloBitrate(500);
        $midBitrateFormat  = (new X264)->setKiloBitrate(1500);
        $highBitrateFormat = (new X264)->setKiloBitrate(3000);

        // open the uploaded video from the right disk...
        FFMpeg::fromDisk($this->video->disk)
            ->open($this->video->path)

        // call the 'exportForHLS' method and specify the disk to which we want to export...
            ->exportForHLS()
            ->toDisk('streamable_videos')

        // we'll add different formats so the stream will play smoothly
        // with all kinds of internet connections...
            ->addFormat($lowBitrateFormat)
            ->addFormat($midBitrateFormat)
            ->addFormat($highBitrateFormat)

        // call the 'save' method with a filename...
            ->save($this->video->id . '.m3u8');

        // update the database so we know the convertion is done!
        $this->video->update([
            'converted_for_streaming_at' => Carbon::now(),
        ]);
    }
}
```

If you want to stream the HLS export in a browser, take a look at this package. It adds HLS support to the excellent Video.js HTML5 video player, even for browsers that don't support HLS natively. When the processing of the video is done you can easily create URLs of the downloadable and streamable versions:

```php
use Illuminate\Support\Facades\Storage;

$downloadUrl = Storage::disk('downloadable_videos')->url($video->id . '.mp4');
$streamUrl = Storage::disk('streamable_videos')->url($video->id . '.m3u8');
```