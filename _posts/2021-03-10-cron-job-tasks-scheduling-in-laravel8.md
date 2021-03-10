---
layout: post
title: "Laravel 8 Cron Job Task Scheduling Tutorial with Example"
date: 2021-03-10 00:07:00 +0800
description: "Laravel 8 Cron Job Task Scheduling Tutorial with Example" # (optional)
img: 2021-03-10-cover-image-of-cron-job-tasks-scheduling.png # Add image post (optional)
fig-caption: "Laravel 8 Cron Job Task Scheduling Tutorial with Example" # Add figcaption (optional)
tags: ['Programming', 'PHP', 'Laravel']
categories: ['Programming', 'PHP', 'Laravel']
---

## Introduction

In this tutorial, we will know how to create a Cron Jon in laravel 8. Making a Cron Job in Laravel 8 is simple and easy. You have to check the entire tutorial gradually with your discretion, devote constant attention to your precedence to learn task scheduling in laravel 8.

Theoretically, Laravel comes with built-in robust task manager, and you can leave all the tasks on its discretion. It gives the precedence to the tasks scheduled by you. It’s better if you have a Linux operating system to make off the Cron Jobs.

- Building a New Application

- Configure Database Connection

- Create New Laravel Artisan Command

- Custom Laravel Command Anatomy

- Register Task Scheduler Command

- Run Laravel Scheduler

## Building a New Application

Let’s evoke the first step by installing a new laravel application, and this step covers the basic imperatives and gives precedence to give you the demo for laravel cron job scheduling.

You can ignore this step if you already have an application downloaded or else, run the following command to create a brand new laravel project.

```sh
composer create-project laravel/laravel laravel-cron-job --prefer-dist
```

Head over to the project directory.

```sh
cd laravel-cron-job
```

## Configure Database Connection

In this type of project, we must give precedence to the database connection, generically it should be configured before getting started. Incorporate the following code in .env file with your database details.

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
```

## Create New Laravel Artisan Command

To create a new artisan laravel command use the ```make: console``` Artisan command, this command gives precedence to generate spontaneous class architecture to get along with.

It is through this application, i would like to demonstrate how to consecutively evoke a new quote daily. Here is the command that should be executed via terminal window.

```sh
php artisan make:command DailyQuote
```

In response to the make: console command, we have formulated a new artisan command, and you can see the conjugated code in DailyQuote.php file inside the app/Console/Commands folder.

```php
<?php

namespace App\Console\Commands;
use Illuminate\Console\Command;

class DailyQuote extends Command
{
    /**
     * The name and signature of the console command.
     *
     * @var string
     */
    protected $signature = 'command:name';

    /**
     * The console command description.
     *
     * @var string
     */
    protected $description = 'Command description';

    /**
     * Create a new command instance.
     *
     * @return void
     */
    public function __construct()
    {
        parent::__construct();
    }

    /**
     * Execute the console command.
     *
     * @return int
     */
    public function handle()
    {
        return 0;
    }
}
```

## Custom Laravel Command Anatomy

Respectively, I’ll try to explain and simplify the conjugated code more easily. Here are the spontaneous variables that came along with the make:console command.

```php
protected $signature = 'command:name';
```

Prefrebarely, the word we used instead of command is `quote` and for the name is `daily` that we will use to run the command.

```php
protected $signature = 'quote:daily';
```

The $description variable is something which should take the precedence with your actual description. This description provides the information about the command, commonly when a user executes the Artisan list command. Change this description, and my description is as follows:

```php
protected $description = 'Respectively send an exclusive quote to everyone daily via email.';
```

When the user invokes the command, It makes the consensus with newly created artisan file and executes the code that resides within the handle method and accomplishes a particular task.

Here is the app/Console/Commands/DailyQuote.php file, It seems when all the code conjugated at one place.

```php
<?php

namespace App\Console\Commands;

use Illuminate\Console\Command;
use App\Models\User;

class DailyQuote extends Command
{
    /**
     * The name and signature of the console command.
     *
     * @var string
     */
    protected $signature = 'quote:daily';

    /**
     * The console command description.
     *
     * @var string
     */
    protected $description = 'Respectively send an exclusive quote to everyone daily via email.';

    /**
     * Create a new command instance.
     *
     * @return void
     */
    public function __construct()
    {
        parent::__construct();
    }  

    /**
     * Execute the console command.
     *
     * @return int
     */
    public function handle()
    {
        $quotes = [
            'Mahatma Gandhi' => 'Live as if you were to die tomorrow. Learn as if you were to live forever.',
            'Friedrich Nietzsche' => 'That which does not kill us makes us stronger.',
            'Theodore Roosevelt' => 'Do what you can, with what you have, where you are.',
            'Oscar Wilde' => 'Be yourself; everyone else is already taken.',
            'William Shakespeare' => 'This above all: to thine own self be true.',
            'Napoleon Hill' => 'If you cannot do great things, do small things in a great way.',
            'Milton Berle' => 'If opportunity doesn’t knock, build a door.'
        ];
         
        // Setting up a random word
        $key = array_rand($quotes);
        $data = $quotes[$key];
         
        $users = User::all();
        foreach ($users as $user) {
            Mail::raw("{$key} -> {$data}", function ($mail) use ($user) {
                $mail->from('digamber@positronx.com');
                $mail->to($user->email)
                    ->subject('Daily New Quote!');
            });
        }
         
        $this->info('Successfully sent daily quote to everyone.');
        
        return 0;
    }
}
```

## Register Task Scheduler Command

So far, with our basic discretion, we completed all the required tasks to develop a Task Scheduler task. Now, It’s time to register the newly created Task Scheduler class in laravel application.

Open app/Console/Kernel.php file and place the following code inside of it.

```php
<?php

namespace App\Console;

use Illuminate\Console\Scheduling\Schedule;
use Illuminate\Foundation\Console\Kernel as ConsoleKernel;

class Kernel extends ConsoleKernel
{
    /**
     * The Artisan commands provided by your application.
     *
     * @var array
     */
    protected $commands = [
        Commands\DailyQuote::class,
    ];

    /**
     * Define the application's command schedule.
     *
     * @param  \Illuminate\Console\Scheduling\Schedule  $schedule
     * @return void
     */
    protected function schedule(Schedule $schedule)
    {
        $schedule->command('quote:daily')
        ->everyMinute();
    }

    /**
     * Register the commands for the application.
     *
     * @return void
     */
    protected function commands()
    {
        $this->load(__DIR__.'/Commands');

        require base_path('routes/console.php');
    }
}
```

Theoretically, we injected DailyQuote command in $commands variable, the schedule() function schedule the command to be invoked daily or on a regular interval. This is the same factory where all ‘Cron Jobs’ related tasks are engineered.

If you run the following command, you will notice that newly created custom artisan command will be displayed on the terminal window with respective description.

```sh
php artisan list
```

Here is the spontaneous response you can expect:

![Cron Job Task Scheduling in Laravel 8]({{site.baseurl}}/assets/img/2021-03-10/2021-03-10-laravel-cron-job-01.png)

From now on, you can trust the following command to schedule your task:

```sh
php artisan quote:daily
```

Generically, On successful execution of command you receive following output:

```sh
Successfully sent daily quote to everyone.
```

### No scheduled commands are ready to run.

> You might get the above error due to execution of the command in advance, for testing purpose you may change the daily() frequency to ->everyMinute();

## Handle Task Scheduler in Laravel

If you have gone through the entire tutorial respectively, then you must know how we defined the frequency of sending a mail daily using the task scheduling method. Code for that Cron Job Task in Laravel 7 was as follows.

```php
protected function schedule(Schedule $schedule)
{
    $schedule->command('quote:daily')
    ->daily();        
}
```

Here are the different Schedule Frequency options list that allows you to schedule your tasks with different rate of occurrences. It should be defined in the Kernel.php file.


|	Method	|	description	|
|:--------	|:-------------	|
|->cron(‘* * * * *’);	|	Run the task on a custom Cron schedule	|
|->everyMinute();	|	Run the task every minute	|
|->everyTwoMinutes();	|	Run the task every two minutes	|
|->everyThreeMinutes();	|	Run the task every three minutes	|
|->everyFourMinutes();	|	Run the task every four minutes	|
|->everyFiveMinutes();	|	Run the task every five minutes	|
|->everyTenMinutes();	|	Run the task every ten minutes	|
|->everyFifteenMinutes();	|	Run the task every fifteen minutes	|
|->everyThirtyMinutes();	|	Run the task every thirty minutes	|
|->hourly();	|	Run the task every hour	|
|->hourlyAt(17);	|	Run the task every hour at 17 minutes past the hour	|
|->everyTwoHours();	|	Run the task every two hours	|
|->everyThreeHours();	|	Run the task every three hours	|
|->everyFourHours();	|	Run the task every four hours	|
|->everySixHours();	|	Run the task every six hours	|
|->daily();	|	Run the task every day at midnight	|
|->dailyAt(’13:00′);	|	Run the task every day at 13:00	|
|->twiceDaily(1, 13);	|	Run the task daily at 1:00 & 13:00	|
|->weekly();	|	Run the task every sunday at 00:00	|
|->weeklyOn(1, ‘8:00’);	|	Run the task every week on Monday at 8:00	|
|->monthly();	|	Run the task on the first day of every month at 00:00	|
|->monthlyOn(4, ’15:00′);	|	Run the task every month on the 4th at 15:00	|
|->monthlyOnLastDay(’15:00′);	|	Run the task on the last day of the month at 15:00	|
|->quarterly();	|	Run the task on the first day of every quarter at 00:00	|
|->yearly();	|	Run the task on the first day of every year at 00:00	|
|->timezone(‘America/New_York’);	|	Set the timezone	|

You can give more precedence to Laravel Scheduling Tasks by examining the [Laravel Documentation](https://laravel.com/docs/8.x/scheduling).

## Run Laravel Scheduler

Theoretically, there are two ways to run scheduler command:

The first method requires you to run the following command, and it’s a manual process.

```sh
php artisan schedule:run
```

On successful execution of command you will receive the following output:

```sh
Running scheduled command: '/usr/local/Cellar/php/7.4.7/bin/php' 'artisan' quote:daily > '/dev/null' 2>&1
```

You can also verify the logs, the path of the logs is as follows **storage/logs/laravel.php**.

In the second method, we automate the task scheduler.

Usually, laravel allow us to run Cron Jobs automatically; you don’t have to execute the command every time. To auto-starting Laravel Scheduler, we require to set Cron Job that executes after every minute.

ssh into your server, get inside your project with cd laravel-project-name and run the following command

```sh
crontab -e
```

It will open the Crontab file, and you need to assimilate the following code in the same file.

Don’t forget to replace /path/to/artisan with the full path to the custom Artisan command of the Laravel project.

```sh
* * * * * cd /your-project-path && php artisan schedule:run >> /dev/null 2>&1
```

## Summary

The primary focus of this tutorial is to understand the boundless opportunities of Laravel 8 Task Scheduler. It brings lots of options to the table, such as creating commands, building logics, and custom task schedulers. Laravel gives utmost precedence to almost every functionality which makes it a robust PHP framework in every sense.
