---
layout: post
title: "How to Create Custom Log Channel With MongoDb in Laravel"
date: 2021-03-14 00:07:00 +0800
description: "How to Create Custom Log Channel With MongoDb in Laravel" # (optional)
img: 2021-03-14-cover-image-of-log-channels-stacks-laravel-header.png # Add image post (optional)
fig-caption: "How to Create Custom Log Channel With MongoDb in Laravel" # Add figcaption (optional)
tags: ['Programming', 'Laravel', 'MongoDB', 'Log']
categories: ['Programming', 'PHP', 'Laravel']
---

## First Comes First

Install MongoDb Driver and PHP Extensions.

## Modify Config

```php

// config/logging.php

use Monolog\Handler\MongoDBHandler;
use App\Logging\MongoDbCustomLogger;

// 此处可以根据需求调整
'channels'=>[
	// other channels, add mongodb channel
	'mongodb' => [
		// 此处必须为 `custom`
	    'driver' => 'custom', 
	    'handler' => MongoDBHandler::class,

	    // 当 `driver` 设置为 custom 时，使用 `via` 配置项所指向的工厂类创建 logger
	    'via' => MongoDbCustomLogger::class, 

	    // 以下 env 配置名可以根据需求调整
	    'server' => env('LOG_MONGO_SERVER', '172.20.0.5'),
	    'port' => env('LOG_MONGO_PORT', 27017),
	    'database' => env('LOG_MONGO_DB', 'logs'),
	    'collection' => env('LOG_MONGO_COLLECTION', 'logs'),
	    'level' => env('LOG_MONGO_LEVEL', 'debug'), // 日志级别
	],
],


```

## Implement MongoDbCustomLogger

```php
namespace App\Logging;

use Monolog\Logger;
use MongoDB\Client;
use Monolog\Handler\MongoDBHandler;
use Monolog\Processor\WebProcessor;

class MongoDbCustomLogger
{
    /**
     * Create a custom Monolog instance.
     *
     * @param  array  $config
     * @return \Monolog\Logger
     */
    public function __invoke(array $config)
    {
    	// 创建 Logger
        $logger = new Logger(); 

        // 创建 Handler
        $handler = new MongoDBHandler( 
        	// 创建 MongoDB 客户端（依赖 mongodb/mongodb）
            new Client('mongodb://' . $config['server'] . $config['port']), 
            $config['database'],
            $config['collection']
        );

        $handler->setLevel($config['level']);

        $logger->pushHandler($handler); // 挂载 Handler
        $logger->pushProcessor(new WebProcessor($_SERVER)); // 记录额外的请求信息

        return $logger;
    }
}
```

## Add Config In .env

```
LOG_MONGO_SERVER=172.20.0.5
LOG_MONGO_PORT=27017
LOG_MONGO_DB=logs
LOG_MONGO_COLLECTION=logs
LOG_MONGO_LEVEL=debug
```

## Test It

```sh
php artisan tinker
```

```php
Log::channel('mongodb')->warning('This is a warning Log');
```

```
use logs
db.logs.find({})
```

and now, you could connect to mongodb, and check the log record in logs database and logs collection.