---
layout: post
title: "Create own custom helper functions / Classes in Laravel"
date: 2021-03-21 00:07:00 +0800
description: "Create own custom helper functions / Classes in Laravel" # (optional)
img: 2021-03-21-cover-image-of-laravel-custom-helpers.png # Add image post (optional)
fig-caption: "Create own custom helper functions / Classes in Laravel" # Add figcaption (optional)
tags: ['Programming', 'Laravel']
categories: ['Programming', 'Laravel']
---

As a programmer, we all need to write some helper functions to reduce repetitive code to speed up our development. Normally Helper functions are the generalised functions to avoid repeating codes.

when it comes to laravel, it contains tremendous amounts of inbuilt helper functions.(my favourites are collect() and dd() ).

[Laravel Helper functions](https://laravel.com/docs/8.x/helpers)

Sometimes we need to develop our very own helper functions which suit our needs. For a simple example, let’s assume we need a few functions to retrieve company details of the current user, logged-in user, and find a user by User Id.

There are three approaches:

1. Autoloading: Create helper functions in a PHP file and load it using Autoload Composer.

1. Using global namespaced functions.

1. Using Service providers to Autoload the helper class

## Autoloading

(Simple and easiest method.)

First of all, you need to create a helper file.

**Step 01. Create a helper.php file inside the laravel app folder.**

Common locations to create the helpers.php files are

- app/helpers.php

- app/Helpers/helpers.php

- app/Http/helpers.php

For this tutorial, let’s go with **app/Helpers/helpers.php**

**Step 2. Open app/Helpers/helpers.php and add your custom function.**

In our example, we wanted the following three functions.

```php
<?php
use App\User;
use App\Company;
use Auth;

if (! function_exists('getUser')) {
    function getUser(int $id): ?object
    { 
       return User::find($id);
    }
}
if (! function_exists('getCurrentUser')) {
    function getCurrentUser() : ?object
    { 
        return Auth::user();

    }
}
if (! function_exists('getUserCompany')) {
    function getUserCompany() : ?object
    { 
        $companyId = Auth::user()->comp_id;
        return Company::find($companyId);    
    }
}
```

Okay, lets load this file using composer autoload

**Step 3. Go to your laravel root directory and open composer.json file, and scroll autoload section. add the helpers.php file path in the file section.**

```json
"autoload": {
    "classmap": [
        "database"
    ],
    "psr-4": {
        "App\\": "app/"
    },
    "files": ["app/Helpers/helpers.php"]
},
"autoload-dev": {
    "classmap": [
        "tests/TestCase.php"
    ]
},
```

Don’t forget to save the files.

**Step 4. Now you need to run dump-autoload, let’s do it by running following command**

```sh
composer dump-autoload
```

That’s it.

Now you can use your helper functions anywhere in your code.

```php
$company = dd(getUserCompany());
$user = dd(getCurrentUser());
$customUser = dd(getUser(10));
```

## Using Global namespace functions

Same as Autoloading approach create a helper PHP file in an appropriate path. For this tutorial, let’s go with **app/Helpers/helpers.php.**

**Step 1. Open app/Helpers/helpers.php**

Add the following code.

```php
<?php
namespace App\Helpers; // Your helpers namespace 

use App\User;
use App\Company;
use Auth;

class UserHelper
{
    public static function getUser(int $userid): ?object
    {
        return User::find($id);
    }
    public static function getCurrentUser(): ?object
    {
         return Auth::user();
    }
    public static function getUserCompany(): ?object
    {
        $companyId = Auth::user()->comp_id;
        return Company::find($companyId);
    }
}
```

**Step 2: Create an alias for the helper file in config/app.php**

Go to your laravel config/app.php scroll down to aliases section.

Add following line to aliases

```php
<?php
'aliases' => [
        'App' => Illuminate\Support\Facades\App::class,
        'Arr' => Illuminate\Support\Arr::class,
        'Artisan' => Illuminate\Support\Facades\Artisan::class,
        ...
        'UserHelper' => App\Helpers\UserHelper::class, //<<this line    
        ...
```

You do not need to run composer dump-autoload.

Now, psr-4 autoload will do the job for you.

Now you can use Helper functions anywhere in the app.

```php
<?php
namespace App\Http\Repositories;

use UserHelper;

class SponsorRepository extends BaseRepository
{
    private $company;
    public function __construct()
    {
       $this->company = UserHelper::getUserCompany();
    }
```

Or use it in the Controller or wherever you want

```php
<?php
namespace App\Http\Controllers;

use UserHelper;

class SponsorController extends Controller
{
    public function getUserCompany() 
    {
        return  UserHelper::getUserCompany();
    }
```

## Using Service providers to Autoload the helper class

(If you have many helper files This will be the easiest way to load the helper classes)

Laravel Service providers are used to autoload classes, lets use this method load our helper class.

**Step 1: Follow step 1 and step 2 from Using Global namespace functions.**

**Step 2: Create a Service provider**

```sh
php artisan make:provider UserHelpServiceProvider
```

**Step 3:Open app/Provider/UserHelpServiceProvider.php and edit the register function and load helper class(es).**

```php
   protected $customHelpers = [
       'StringHelper',
       'UserHelper'
   ];
   public function register()
   {
        foreach ($this->customHelpers as $helper) {
            require_once(app_path().'/Helpers/'.$helper.'.php');
        }
   }
```

**Step 4: Create an alias for the helper file in config/app.php**

Go to your laravel config/app.php scroll down to providers array and Add the following lines.

```php
'providers' => [
        /*
        * Laravel Framework Service Providers...
        */
        Illuminate\Auth\AuthServiceProvider::class,
        Illuminate\Broadcasting\BroadcastServiceProvider::class,
        Illuminate\Bus\BusServiceProvider::class,
        Illuminate\Cache\CacheServiceProvider::class,

        App\Providers\EventServiceProvider::class,
        App\Providers\RouteServiceProvider::class,
        ...
        App\Providers\HelperServiceProvider::class, // add this line
        ...
]
```

** Step 5: Create an alias (Refer to Using Global namespace functions. Step 2)**

That's it :)