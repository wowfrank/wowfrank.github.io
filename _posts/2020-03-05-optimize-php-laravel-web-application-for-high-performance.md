---
layout: post
title: "How to Optimize PHP Laravel Web Application for High Performance?"
date: 2020-03-05 00:01:00 +0800
description: "How to Optimize PHP Laravel Web Application for High Performance?" # (optional)
img: laravel-1-1091x628.webp # Add image post (optional)
fig-caption: "How to Optimize PHP Laravel Web Application for High Performance?" # Add figcaption (optional)
tags: ['PHP','Programming','Optimize']
categories: ['Programming', 'PHP']
---

> Laravel is many things. But fast isn’t one of them. Let’s learn some tricks of the trade to make it go faster!

No PHP developer is untouched by Laravel these days. They’re either a junior or mid-level developer who love the rapid development Laravel offers, or they’re a senior developer who’s being forced to learn Laravel because of market pressures.

Either way, there’s no denying that Laravel has revitalized the PHP ecosystem (I, for sure, would’ve left the PHP world long ago if Laravel wasn’t there).

However, since Laravel bends over backward to make things easy for you, it means that underneath it’s doing tons and tons of work to make sure you have a comfortable life as a developer. All the “magical” features of Laravel that just seem to work have layers upon layers of code that needs to be whipped up each time a feature runs. Even a simple Exception trace how deep the rabbit hole is (notice where the error starts, all the way down to the main kernel):

<div align="center"><div markdown='1'>
![]({{site.baseurl}}/assets/img/laravel-2-966x628.webp)
</div></div>

For what seems to be a compilation error in one of the views, there are 18 function calls to trace. I’ve personally come across 40, and there could easily be more if you’re using other libraries and plugins.

Point being, by default this layers upon layers of code, make Laravel slow.

## How slow is Laravel?

Honestly, it’s plain impossible to answer this question for several reasons.

**First**, there’s no accepted, objective, sensible standard for measuring the speed of Web apps. Faster or slower compared to what? Under what conditions?

**Second**, a Web app depends on so many things (database, filesystem, network, cache, etc.) that it’s plain silly to talk about speed. A very fast Web app with a very slow database is a very slow web app. 🙂

But this uncertainty is precisely why benchmarks are popular. Even though they mean nothing (see this and this), they provide some frame of reference and help us from going mad. Therefore, with several pinches of salt ready, let’s get a wrong, rough idea of speed among PHP frameworks.

Going by this rather respectable GitHub source, here’s how the PHP frameworks line up when compared:

<div align="center"><div markdown='1'>
![PHP Framework Benchmark]({{site.baseurl}}/assets/img/php-frameworks-speed-comparison.webp)
</div></div>

You may not even notice Laravel here (even if you squint real hard) unless you cast your case right to the end of the tail. Yes, dear friends, Laravel comes last! Now, granted, most of these “frameworks” are not very practical or even useful, but it does tell us how sluggish Laravel is when compared to other more popular ones.

Normally, this “slowness” doesn’t feature in applications because our everyday web apps rarely hit high numbers. But once they do (say, upwards of 200-500 concurrency), the servers begin to choke and die. It’s the time when even throwing more hardware at the problem doesn’t cut it, and infrastructure bills climb so fast that your high ideals of cloud computing come crashing down.

<div align="center"><div markdown='1'>
![sad]({{site.baseurl}}/assets/img/laravel-optimize-performance-sad.jpg)
</div></div>

But hey, cheer up! This article isn’t about what can’t be done, but about what can be done. 🙂

Good news is, you can do a lot to make your Laravel app go faster. Several times fast. Yes, no kidding. You can make the same codebase go ballistic and save several hundred dollars on infrastructure/hosting bills every month. How? Let’s get to it.

Four types of optimizations

In my opinion, optimization can be done on four distinct levels (when it comes to PHP applications, that is):

- **Language-level**: This means you use a faster version of the language and avoid specific features/styles of coding in the language that makes your code slow.
- **Framework-level**: These are the things we will be covering in this article.
- **Infrastructure-level**: Tuning your PHP process manager, web server, database, etc.
- **ardware-level**: Moving to a better, faster, more powerful hardware hosting provider.

All of these types of optimizations have their place (for instance, php-fpm optimization is pretty critical and powerful). But the focus of this article will be optimizations purely of type 2: those related to the framework.

By the way, there’s no rationale behind the numbering, and it’s not an accepted standard. I just made these up. Please don’t ever quote me and say, “We need type-3 optimization on our server,” or your team lead will kill you, find me, and then kill me as well. 😀

And now, finally, we arrive at the promised land.

## Be aware of n+1 database queries

The n+1 query problem is a common one when ORMs are used. Laravel has its powerful ORM called Eloquent, which is so beautiful, so convenient, that we often forget to look at what’s going on.

Consider a very common scenario: displaying the list of all orders placed by a given list of customers. This is pretty common in e-commerce systems and any reporting interfaces in general where we need to display all entities related to some entities.

In Laravel, we might imagine a controller function that does the job like this:

```php
class OrdersController extends Controller
{
    // ...

    public function getAllByCustomers(Request $request, array $ids) {
        $customers = Customer::findMany($ids);
        $orders = collect(); // new collection

        foreach ($customers as $customer) {
            $orders = $orders->merge($customer->orders);
        }

        return view('admin.reports.orders', ['orders' => $orders]);
    }
}
```

Sweet! And more importantly, elegant, beautiful. 🤩🤩

Unfortunately, it’s a disastrous way to write code in Laravel.

Here’s why.

When we ask the ORM to look for the given customers, a SQL query like this gets generated:

```sql
SELECT * FROM customers WHERE id IN (22, 45, 34, . . .);
```

Which is exactly as expected. As a result, all the returned rows get stored in the collection $customers inside the controller function.

Now we loop over each customer one by one and get their orders. This executes the following query . . .

```sql
SELECT * FROM orders WHERE customer_id = 22;
```

. . . as many times as there are customers.

In other words, if we need to get the order data for 1000 customers, the total number of database queries executed will be 1 (for fetching all the customers’ data) + 1000 (for fetching order data for each customer) = 1001. This is where the name n+1 comes from.

Can we do better? Certainly! By using what’s known as eager loading, we can force the ORM to perform a JOIN and return all the needed data in a single query! Like this:

```php
$orders = Customer::findMany($ids)->with('orders')->get();
```

The resulting data structure is a nested one, sure, but the order data can be easily extracted. The resulting single query, in this case, is something like this:

```sql
SELECT * FROM customers INNER JOIN orders ON customers.id = orders.customer_id WHERE customers.id IN (22, 45, . . .);
```

A single query is, of course, better than a thousand extra queries. Imagine what would happen if there were 10,000 customers to process! Or God forbid if we also wanted to display the items contained in every order! Remember, the name of the technique is eager loading, and it’s almost always a good idea.

## Cache the configuration!

One of the reasons for Laravel’s flexibility is the tons of configuration files that are part of the framework. Want to change how/where the images are stored?

Well, just change the config/filesystems.php file (at least as of writing). Want to work with multiple queue drivers? Feel free to describe them in config/queue.php. I just counted and found that there are 13 configuration files for different aspects of the framework, ensuring you won’t be disappointed no matter what you want to change.

<div align="center"><div markdown='1'>
![many-tools]({{site.baseurl}}/assets/img/laravel-optimize-performance-many-tools.jpg)
</div></div>

Given the nature of PHP, every time a new Web request comes in, Laravel wakes up, boots everything, and parses all of these configuration files to figure out how to do things differently this time. Except that it’s stupid if nothing has changed in the last few days! Rebuilding the config on every request is a waste that can be (actually, must be) avoided, and the way out is a simple command that Laravel offers:

```bash
php artisan config:cache
```

What this does is combine all the available config files into a single one and cache is somewhere for fast retrieval. The next time there’s a Web request, Laravel will simply read this single file and get going.

That said, configuration caching is an extremely delicate operation that can blow up in your face. The biggest gotcha is that once you’ve issued this command, the env() function calls from everywhere except the config files will return null!

It does make sense when you think about it. If you use configuration caching, you’re telling the framework, “You know what, I think I’ve set things up nicely and I’m 100% sure I don’t want them to change.” In other words, you’re expecting the environment to stay static, which is what .env files are for.

With that said, here are some iron-clad, sacred, unbreakable rules of configuration caching:

1. Do it only on a production system.
1. Do it only if you’re really, really sure you want to freeze the configuration.
1. In case something goes wrong, undo the setting with php artisan cache:clear
1. Pray that the damage done to the business wasn’t significant!

## Reduce autoloaded services

To be helpful, Laravel loads a ton of services when it wakes up. These are available in the config/app.php file as part of the 'providers' array key. Let’s have a look at what I have in my case:

```php
/*
    |--------------------------------------------------------------------------
    | Autoloaded Service Providers
    |--------------------------------------------------------------------------
    |
    | The service providers listed here will be automatically loaded on the
    | request to your application. Feel free to add your own services to
    | this array to grant expanded functionality to your applications.
    |
    */

    'providers' => [

        /*
         * Laravel Framework Service Providers...
         */
        Illuminate\Auth\AuthServiceProvider::class,
        Illuminate\Broadcasting\BroadcastServiceProvider::class,
        Illuminate\Bus\BusServiceProvider::class,
        Illuminate\Cache\CacheServiceProvider::class,
        Illuminate\Foundation\Providers\ConsoleSupportServiceProvider::class,
        Illuminate\Cookie\CookieServiceProvider::class,
        Illuminate\Database\DatabaseServiceProvider::class,
        Illuminate\Encryption\EncryptionServiceProvider::class,
        Illuminate\Filesystem\FilesystemServiceProvider::class,
        Illuminate\Foundation\Providers\FoundationServiceProvider::class,
        Illuminate\Hashing\HashServiceProvider::class,
        Illuminate\Mail\MailServiceProvider::class,
        Illuminate\Notifications\NotificationServiceProvider::class,
        Illuminate\Pagination\PaginationServiceProvider::class,
        Illuminate\Pipeline\PipelineServiceProvider::class,
        Illuminate\Queue\QueueServiceProvider::class,
        Illuminate\Redis\RedisServiceProvider::class,
        Illuminate\Auth\Passwords\PasswordResetServiceProvider::class,
        Illuminate\Session\SessionServiceProvider::class,
        Illuminate\Translation\TranslationServiceProvider::class,
        Illuminate\Validation\ValidationServiceProvider::class,
        Illuminate\View\ViewServiceProvider::class,

        /*
         * Package Service Providers...
         */

        /*
         * Application Service Providers...
         */
        App\Providers\AppServiceProvider::class,
        App\Providers\AuthServiceProvider::class,
        // App\Providers\BroadcastServiceProvider::class,
        App\Providers\EventServiceProvider::class,
        App\Providers\RouteServiceProvider::class,

    ],
```

Once again, I counted, and there are 27 services listed! Now, you may need all of them, but it’s unlikely.

For instance, I happen to be building a REST API at the moment, which means I don’t need the Session Service Provider, View Service Provider, etc. And since I’m doing a few things my way and not following the framework defaults, I can also disable Auth Service Provider, Pagination Service Provider, Translation Service Provider, and so on. All in all, almost half of these are unnecessary for my use case.

Take a long, hard look at your application. Does it need all of these service providers? But for God’s sake, please don’t blindly comment out these services and push to production! Run all the tests, check things manually on dev and staging machines, and be very very paranoid before you pull the trigger. 🙂

## Be wise with middleware stacks

When you need some custom processing of the incoming Web request, creating a new middleware is the answer. Now, it’s tempting to open app/Http/Kernel.php and stick the middleware in the web or api stack; that way, it becomes available across the app and if it’s not doing something intrusive (like logging or notifying, for example).

However, as the app grows, this collection of global middleware can become a silent burden on the app if all (or majority) of these are present in every request, even if there’s no business reason for that.

In other words, be careful of where you add/apply a new middleware. It can be more convenient to add something globally, but the performance penalty is very high in the long run. I know the pain you’d have to undergo if you were to selectively apply middleware every time there’s a new change, but it’s a pain I’d willingly take and recommend!

## Avoid the ORM (at times)

While Eloquent makes many aspects of DB interaction pleasurable, it comes at the cost of speed. Being a mapper, the ORM not only has to fetch records from the database but also instantiate the model objects and hydrate (fill them in) them with column data.

So, if you do a simple $users = User::all() and there are, say, 10,000 users, the framework will fetch 10,000 rows from the database and internally do 10,000 new User() and fill their properties with the relevant data. This is massive amounts of work being done behind the scenes, and if the database is where you’re application is becoming a bottleneck, bypassing the ORM is a good idea at times.

This is especially true for complex SQL queries, where you’d have to jump a lot of hoops and write closures upon closures and still end up with an efficient query. In such cases, doing a DB::raw() and writing the query by hand is preferred.

Going by this performance study, even for simple inserts Eloquent is much slower as the number of records rises:

<div align="center"><div markdown='1'>
![eloquent-vs-raw-sql-insert]({{site.baseurl}}/assets/img/eloquent-vs-raw-sql-insert.webp)
</div></div>

## Use caching as much as possible

One of the best-kept secrets of Web application optimization is caching.

For the uninitiated, caching means precomputing and storing expensive results (expensive in terms of CPU and memory usage), and simply returning them when the same query is repeated.

For instance, in an e-commerce store, it might come across that of the 2 million products, most of the time people are interested in those that are freshly stocked, within a certain price range, and for a particular age group. Querying the database for this information is wasteful — since the query doesn’t change often, it’s better to store these results somewhere we can access quickly.

Laravel has built-in support for several types of caching. In addition to using a caching driver and building the caching system from the ground up, you might want to use some Laravel packages that facilitate model caching, query caching, etc.

But do note that beyond a certain simplified use case, prebuilt caching packages can cause more problems than they solve.

## Prefer in-memory caching

When you cache something in Laravel, you have several options of where to store the resulting computation that needs to be cached. These options are also known as cache drivers. So, while it’s possible and perfectly reasonable to use the file system for storing cache results, it’s not really what caching is meant to be.

Ideally, you want to use an in-memory (living in the RAM entirely) cache like Redis, Memcached, MongoDB, etc., so that under higher loads, caching serves a vital use rather than becoming a bottleneck itself.

Now, you might think that having an SSD disk is almost the same as using a RAM stick, but it’s not even close. Even informal benchmarks show that RAM outperforms SSD by 10-20 times when it comes to speed.

My favorite system when it comes to caching is Redis. It’s ridiculously fast (100,000 read operations per second are common), and for very large cache systems, can be evolved into a cluster easily.

## Cache the routes

Just like the application config, the routes don’t change much over time and are an ideal candidate for caching. This is especially true if you can’t stand large files like me and end up splitting your web.php and api.php over several files. A single Laravel command packs up all the available routes and keeps them handy for future access:

```bash
php artsan route:cacheCopy
```

And when you do end up adding or changing routes, simply do:

```bash
php artisan route:clear
```

## Image optimization and CDN

Images are the heart-and-soul of most Web applications. Coincidentally, they’re also the largest consumers of bandwidth and one of the biggest reasons for slow apps/websites. If you simply store the uploaded images naively on the server and send them back in HTTP responses, you’re letting a massive optimization opportunity slip by.

My first recommendation is not to store images locally — there’s the problem of data loss to deal with, and depending on which geographic region your customer is in, data transfer can be painfully slow.

Instead, go for a solution like Cloudinary that automatically resizes and optimizes images on the fly.

If that’s not possible, use something like Cloudflare to cache and serve images while they’re stored on your server.

And if even that is not possible, tweaking your web server software a little to compress assets and direct the visitor’s browser to cache things, makes a lot of difference. Here’s how a snippet of Nginx configuration would look like:

```sh
server {

   # file truncated

    # gzip compression settings
    gzip on;
    gzip_comp_level 5;
    gzip_min_length 256;
    gzip_proxied any;
    gzip_vary on;

   # browser cache control
   location ~* \.(ico|css|js|gif|jpeg|jpg|png|woff|ttf|otf|svg|woff2|eot)$ {
         expires 1d;
         access_log off;
         add_header Pragma public;
         add_header Cache-Control "public, max-age=86400";
    }
}
```

I’m aware that image optimization has nothing to do with Laravel, but it’s such a simple and powerful trick (and is so often neglected) that couldn’t help myself.

## Autoloader optimization

Autoloading is a neat, not-so-old feature in PHP that arguably saved the language from doom. That said, the process of finding and loading the relevant class by deciphering a given namespace string takes time and can be avoided in production deployments where high performance is desirable. Once again, Laravel has a single-command solution to this:

```bash
composer install --optimize-autoloader --no-devCopy
```

## Make friends with queues

Queues are how you process things when there are many of them, and each of them takes a few milliseconds to complete. A good example is sending emails — a widespread use case in web apps is to shoot off a few notification emails when a user performs some actions.

For instance, in a newly launched product, you might want the company leadership (some 6-7 email addresses) to be notified whenever someone places an order above a certain value. Assuming that your email gateway can respond to your SMTP request in 500ms, we’re talking of a good 3-4 second wait for the user before the order confirmation kicks in. A really bad piece of UX, I’m sure you’ll agree.

The remedy is to store jobs as they come in, tell the user that everything went well, and process them (a few seconds) later. If there’s an error, the queued jobs can be retried a few times before they’re declared to have failed.

<div align="center"><div markdown='1'>
![queued-jobs]({{site.baseurl}}/assets/img/laravel-optimize-performance-queued-jobs.webp)
</div></div>

While a queueing system complicates the setup a little (and adds some monitoring overhead), it’s indispensable in a modern Web application.

## 
Asset optimization (Laravel Mix)

For any front-end assets in your Laravel application, please make sure there’s a pipeline that compiles and minifies all the asset files. Those who are comfortable with a bundler system like Webpack, Gulp, Parcel, etc., don’t need to bother, but if you’re not doing this already, Laravel Mix is a solid recommendation.

Mix is a lightweight (and delightful, in all honesty!) wrapper around Webpack which takes care of all your CSS, SASS, JS, etc., files for production. A typical .mix.js file can be as small as this and still work wonders:

```javascript
const mix = require('laravel-mix');

mix.js('resources/js/app.js', 'public/js')
    .sass('resources/sass/app.scss', 'public/css');
```

This automatically takes care of imports, minification, optimization and the whole shebang when you’re ready for production and run npm run production. Mix takes care of not just traditional JS and CSS files, but also Vue and React components that you might have in your application workflow.

## Conclusion

Performance optimization is more art than science — knowing how and how much to do is important than what to do. That said, there’s no end to how much and what all you can optimize in a Laravel application.

But whatever you do, I’d like to leave you with some parting advice — optimization should be done when there’s a solid reason, and not because it sounds good or because you’re paranoid about the app performance for 100,000+ users while in reality there are only 10.

If you’re not sure whether you need to optimize your app or not, you don’t need to kick the proverbial hornets’ nest. A working app that feels boring but does exactly what it has to is ten times more desirable than an app that has been optimized into a mutant hybrid supermachine but falls flat now and then.