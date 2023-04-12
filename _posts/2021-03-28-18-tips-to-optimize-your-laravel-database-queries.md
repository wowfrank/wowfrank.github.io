---
layout: post
title: "18 Tips to Optimize Your Laravel Database Queries"
date: 2021-03-28 00:07:00 +0800
description: "18 Tips to Optimize Your Laravel Database Queries" # (optional)
img: 2021-03-28-cover-image-of-18-tips-to-optimize-your-laravel-database-queries.png # Add image post (optional)
fig-caption: "18 Tips to Optimize Your Laravel Database Queries" # Add figcaption (optional)
tags: ['Programming', 'Laravel', 'PHP']
categories: ['Programming', 'PHP', 'Laravel']
---

If your application is running slow or making a lot of database queries, follow the below performance optimization tips to improve your application loading time.

## Retrieving Large Datasets

This tip mainly focuses on improving the memory usage of your application when dealing with large datasets.

If your application needs to process a large set of records, instead of retrieving all at once, you can retrieve a
a subset of results and process them in groups.

To retrieve many results from a table called posts, we would usually do like below.

```php
$posts = Post::all(); // when using eloquent
$posts = DB::table('posts')->get(); // when using query builder

foreach ($posts as $post){
 // Process posts
}
```

The above examples will retrieve all the records from the posts table and process them. What if this table has 1 million rows? We will quickly run out of memory.

To avoid issues when dealing with large datasets, we can retrieve a subset of results and process them as below.

### Option 1: Using chunk

```php
// when using eloquent
$posts = Post::chunk(100, function($posts){
    foreach ($posts as $post){
     // Process posts
    }
});

// when using query builder
$posts = DB::table('posts')->chunk(100, function ($posts){
    foreach ($posts as $post){
     // Process posts
    }
});
```

The above example retrieves 100 records from the posts table, processes them, retrieves another 100 records, and processes them. This iteration will continue until all the records are processed.

This approach will create more database queries but be more memory efficient. Usually, the processing of large datasets should be doe in the background. So it is ok to make more queries when running in the background to avoid running out of memory when processing large datasets.

### Option 2: Using cursor

```php
// when using eloquent
foreach (Post::cursor() as $post){
   // Process a single post
}

// when using query builder
foreach (DB::table('posts')->cursor() as $post){
   // Process a single post
}
```

The above example will make a single database query, retrieve all the records from the table, and hydrate Eloquent models one by one. This approach will make only one database query to retrieve all the posts. But uses php generator to optimize the memory usage.

### when can you use this?

Though this greatly optimizes the memory usage on the application level, Since we are retrieving all the entries from a table, the memory usage on the database instance will still be higher.

It is better to use a cursor If your web app running your application has less memory, and the database instance has more memory. However, if your database instance does not have enough memory, it is better to stick to chunks.

### option 3: Using chunkById

```php
// when using eloquent
$posts = Post::chunkById(100, function($posts){
    foreach ($posts as $post){
     // Process posts
    }
});

// when using query builder
$posts = DB::table('posts')->chunkById(100, function ($posts){
    foreach ($posts as $post){
     // Process posts
    }
});
```

The major difference between chunk and chunkById is that chunk retrieves based on offset and limit. Whereas
chunkById retrieves database results based on an id field. This id field usually be an integer field, and in most cases it would be an auto-incrementing field.

The queries made by chunk and chunkById were as follows.

**chunk**

```sql
select * from posts offset 0 limit 100
select * from posts offset 101 limit 100
```

**chunkById**

```sql
select * from posts order by id asc limit 100
select * from posts where id > 100 order by id asc limit 100
```

Generally, using a limit with offset is slower, and we should try to avoid using it. This article explains in detail the problem with using offset.

As chunkById is using the id field which is an integer, and the query is using a where clause, the query will be much faster.

### When can you use chunkById?

If your database table has a primary key column column, which is an auto-incrementing field.

## Select only the columns you need

Usually to retrieve results from a database table, we would do the following.

```php
$posts = Post::find(1); //When using eloquent
$posts = DB::table('posts')->where('id','=',1)->first(); //When using query builder
```

The above code will result in a query as below

```sql
select * from posts where id = 1 limit 1
```

As you can see, the query is doing a select \*. This means it is retrieving all the columns from the database table.

This is fine if we really need all the columns from the table.

Instead, if we need only specific columns(id, title), we can retrieve only those columns as below.

```php
$posts = Post::select(['id','title'])->find(1); //When using eloquent
$posts = DB::table('posts')->where('id','=',1)->select(['id','title'])->first(); //When using query builder
```

The above code will result in a query as below

```sql
select id,title from posts where id = 1 limit 1
```

## Use pluck when you need exactly one or two columns from the database

> This tip focuses more on the time spent after the results are retrieved from the database. This does not affect the actual query time.

As I mentioned above, to retrieve specific columns, we would do

```php
$posts = Post::select(['title','slug'])->get(); //When using eloquent
$posts = DB::table('posts')->select(['title','slug'])->get(); //When using query builder
```

When the above code is executed, it does the following behind the scenes.

- Executes select title, slug from posts query on the database

- Creates a new Post model object for each row it retrieved(For query builder, it creates a PHP standard object)

- Creates a new collection with the Post models

- Returns the collection

Now, to access the results, we would do

```php
foreach ($posts as $post){
    // $post is a Post model or php standard object
    $post->title;
    $post->slug;
}
```

The above approach has an additional overhead of hydrating Post model for each and every row and creating a collection for these objects. This would be best if you really need the Post model instance instead of the data.

But if all that you need is those two values, you can do the following.

```php
$posts = Post::pluck('title', 'slug'); //When using eloquent
$posts = DB::table('posts')->pluck('title','slug'); //When using query builder
```

When the above code is executed, it does the following behind the scenes.

- Executes select title, slug from posts query on the database

- Creates an array with title as array value and slug as array key.

- Returns the array(array format: [ slug => title, slug => title ])

Now, to access the results, we would do

```php
foreach ($posts as $slug => $title){
    // $title is the title of a post
    // $slug is the slug of a post
}
```

If you want to retrieve only one column, you can do

```php
$posts = Post::pluck('title'); //When using eloquent
$posts = DB::table('posts')->pluck('title'); //When using query builder
foreach ($posts as  $title){
    // $title is the title of a post
}
```

The above approach eliminates the creation of Post objects for every row. Thus reducing the memory usage and
time spent on processing the query results.

> I would recommend using the above approach on new code only. I personally feel going back and refactoring your code to follow the above tip is not worthy of the time spent on it. Refactor existing code only if your code is processing large datasets or if you have free time to spare.

## Count rows using a query instead of a collection

To count the total no of rows in a table, we would normally do

```php
$posts = Post::all()->count(); //When using eloquent
$posts = DB::table('posts')->get()->count(); //When using query builder
```

This will generate the following query

```sql
select * from posts
```

The above approach will retrieve all the rows from the table, load them into a collection object, and counts the results. This works fine when there are less rows in the database table. But we will quickly run out of memory as the table grows.

Instead of the above approach, we can directly count the total no of rows on the database itself.

```php
$posts = Post::count(); //When using eloquent
$posts = DB::table('posts')->count(); //When using query builder
```

This will generate the following query

```sql
select count(*) from posts
```

Counting rows in sql is a slow process and performs poorly when the database table has so many rows. It is better to avoid counting of rows as much as possible.

## Avoid N+1 queries by eager loading relationship

You might have heard of this tip a million times. So I will keep it as short and simple as possible. Let's assume you have the following scenario

```php
class PostController extends Controller
{
    public function index()
    {
        $posts = Post::all();
        return view('posts.index', ['posts' => $posts ]);
    }
}
// posts/index.blade.php file

@foreach($posts as $post)
    <li>
        <h3>{{ $post->title }}</h3>
        <p>Author: {{ $post->author->name }}</p>
    </li>
@endforeach
```

The above code is retrieving all the posts and displaying the post title and its author on the webpage, and it assumes you have an author relationship on your post model.

Executing the above code will result in running following queries.

```sql
select * from posts // Assume this query returned 5 posts
select * from authors where id = { post1.author_id }
select * from authors where id = { post2.author_id }
select * from authors where id = { post3.author_id }
select * from authors where id = { post4.author_id }
select * from authors where id = { post5.author_id }
```

As you can see, we have one query to retrieve posts, and 5 queries to retrieve authors of the posts(Since we assumed we have 5 posts.) So for every post it retrieved, it is making one separate query to retrieve its author.

So if there are N number of posts, it will make N+1 queries( 1 query to retrieve posts and N queries to retrieve the author for each post). This is commonly known as N+1 query problem.

To avoid this, eager load the author's relationship on posts as below.

```php
$posts = Post::all(); // Avoid doing this
$posts = Post::with(['author'])->get(); // Do this instead
```

Executing the above code will result in running the following queries.

```sql
select * from posts // Assume this query returned 5 posts
select * from authors where id in( { post1.author_id }, { post2.author_id }, { post3.author_id }, { post4.author_id }, { post5.author_id } )
```

## Eager load nested relationship

From the above example, consider the author belongs to a team, and you wish to display the team name as well. So in the blade file you would do as below.

```php
@foreach($posts as $post)
    <li>
        <h3>{{ $post->title }}</h3>
        <p>Author: {{ $post->author->name }}</p>
        <p>Author's Team: {{ $post->author->team->name }}</p>
    </li>
@endforeach
```

Now doing the below

```php
$posts = Post::with(['author'])->get();
```

Will result in following queries

```sql
select * from posts // Assume this query returned 5 posts
select * from authors where id in( { post1.author_id }, { post2.author_id }, { post3.author_id }, { post4.author_id }, { post5.author_id } )
select * from teams where id = { author1.team_id }
select * from teams where id = { author2.team_id }
select * from teams where id = { author3.team_id }
select * from teams where id = { author4.team_id }
select * from teams where id = { author5.team_id }
```

As you can see, even though we are eager loading authors relationship, it is still making more queries. Because we are not eager loading the team relationship on authors.

We can fix this by doing the following.

```php
$posts = Post::with(['author.team'])->get();
```

Executing the above code will result in running following queries.

```sql
select * from posts // Assume this query returned 5 posts
select * from authors where id in( { post1.author_id }, { post2.author_id }, { post3.author_id }, { post4.author_id }, { post5.author_id } )
select * from teams where id in( { author1.team_id }, { author2.team_id }, { author3.team_id }, { author4.team_id }, { author5.team_id } )
```

By eager loading the nested relationship, we reduce the total number of queries from 11 to 3.

## Do not load belongsTo relationship if you just need its id

Imagine you have two tables posts and authors. Posts table has a column author_id which represents a belongsTo relationship on the authors table.

To get the author id of a post, we would normally do

```php
$post = Post::findOrFail(<post id>);
$post->author->id; 
```

This would result in two queries being executed.

```sql
select * from posts where id = <post id> limit 1
select * from authors where id = <post author id> limit 1
```

Instead, you can directly get the author id by doing the following.

```php
$post = Post::findOrFail(<post id>);
$post->author_id; // posts table has a column author_id which stores id of the author
```

When can I use the above approach?

You can use the above approach when you are confident that a row always exists in authors table if it is referenced in posts table.

## Avoid unnecessary queries

Oftentimes, we make database queries that are not necessary. Consider the below example.

```php
<?php

class PostController extends Controller
{
    public function index()
    {
        $posts = Post::all();
        $private_posts = PrivatePost::all();
        return view('posts.index', ['posts' => $posts, 'private_posts' => $private_posts ]);
    }
}
```

The above code is retrieving rows from two different tables(ex: posts, private_posts) and passing them to view.

The view file looks as below.

```php
// posts/index.blade.php

@if( request()->user()->isAdmin() )
    <h2>Private Posts</h2>
    <ul>
        @foreach($private_posts as $post)
            <li>
                <h3>{{ $post->title }}</h3>
                <p>Published At: {{ $post->published_at }}</p>
            </li>
        @endforeach
    </ul>
@endif

<h2>Posts</h2>
<ul>
    @foreach($posts as $post)
        <li>
            <h3>{{ $post->title }}</h3>
            <p>Published At: {{ $post->published_at }}</p>
        </li>
    @endforeach
</ul>
```

As you can see above, $private_posts is visible to only a user who is an admin. Rest all the users cannot see
these posts.

The problem here is, when we are doing

```php
$posts = Post::all();
$private_posts = PrivatePost::all();
```

We are making two queries. One to get the records from posts table and another to get the records
from private_posts table.

Records from private_posts table are visible only to the admin user. But we are still making the query to retrieve these records for all the users even though they are not visible.

We can modify our logic to below to avoid this extra query.

```php
$posts = Post::all();
$private_posts = collect();
if( request()->user()->isAdmin() ){
    $private_posts = PrivatePost::all();
}
```

By changing our logic to the above, we are making two queries for the admin user and one query for all other users.

##  Merge similar queries

We sometimes need to make queries to retrieve different kinds of rows from the same table.

```php
$published_posts = Post::where('status','=','published')->get();
$featured_posts = Post::where('status','=','featured')->get();
$scheduled_posts = Post::where('status','=','scheduled')->get();
```

The above code is retrieving rows with a different status from the same table. The code will result in making
following queries.

```sql
select * from posts where status = 'published'
select * from posts where status = 'featured'
select * from posts where status = 'scheduled'
```

As you can see, it is making three different queries to the same table to retrieve the records. We can refactor this code to make only one database query.

```php
$posts =  Post::whereIn('status',['published', 'featured', 'scheduled'])->get();
$published_posts = $posts->where('status','=','published');
$featured_posts = $posts->where('status','=','featured');
$scheduled_posts = $posts->where('status','=','scheduled');
```

```sql
select * from posts where status in ( 'published', 'featured', 'scheduled' )
```

The above code is making one query to retrieve all the posts which has any of the specified status and creating separate collections for each status by filtering the returned posts by their status. So we will still have three different variables with their status and will be making only one query.

## Add index to frequently queried columns

If you are making queries by adding a where condition on a string based column, it is better to add an index to the column. Queries are much faster when querying rows with an index column.

```php
$posts = Post::where('status','=','published')->get();
```

In the above example, we are querying records by adding a where condition to the status column. We can improve the performance of the query by adding the following database migration.

```php
Schema::table('posts', function (Blueprint $table) {
   $table->index('status');
});
```

## Use simplePaginate instead of Paginate

When paginating results, we would usually do

```php
$posts = Post::paginate(20);
```

This will make two queries, the first to retrieve the paginated results and a second to count the total no of rows in the table. Counting rows in a table is a slow operation and will negatively affect the query performance.

So why does laravel count the total no of rows?

To generate pagination links, Laravel counts the total no of rows. So, when the pagination links are generated, you know before-hand, how many pages will be there, and what is the past page number. So you can navigate to what ever the page you want easily.

On the other hand, doing simplePaginate will not count the total no of rows and the query will be much faster than the paginate approach. But you will lose the ability to know the last page number and able to jump to different pages.

If your database table has so many rows, it is better to avoid paginate and do simplePaginate instead.

```php
$posts = Post::paginate(20); // Generates pagination links for all the pages
$posts = Post::simplePaginate(20); // Generates only next and previous pagination links
```

### When to use paginate vs simple paginate?

Look at the below comparison table and determine if paginate or simple paginate is right for you

|       |   paginate / simplePaginate   |
|:------|:------------------------------|
|database table has only few rows and does not grow large	|paginate / simplePaginate|
|database table has so many rows and grows quickly	|simplePaginate|
|it is mandatory to provide the user option to jump to specific pages	|paginate|
|it is mandatory to show the user total no of results	|paginate|
|not actively using pagination links	|simplePaginate|
|UI/UX does not affect from switching numbered pagination links to next / previous pagination links	|simplePaginate|
|Using "load more" button or "infinite scrolling" for pagination	|simplePaginate|

## Avoid using leading wildcards(LIKE keyword)

When trying to query results which match a specific pattern, we would usually go with

```sql
select * from table_name where column like %keyword%
```

The above query will result in a full table scan. If We know the keyword occurs at the beginning of the column value,

We can query the results as below.

```sql
select * from table_name where column like keyword%
```

##  avoid using SQL functions in where clause

It is always better to avoid SQL functions in where clause as they result in full table scan. Let's look at the below example. To query results based on the certain date, we would usually do

```php
$posts = POST::whereDate('created_at', '>=', now() )->get();
```

This will result in a query similar to below

```sql
select * from posts where date(created_at) >= 'timestamp-here'
````

The above query will result in a full table scan, because the where condition isn't applied until the date function is evaluated.

We can refactor this to avoid the date sql function as below

```php
$posts = Post::where('created_at', '>=', now() )->get();
```

```sql
select * from posts where created_at >= 'timestamp-here'
```

## avoid adding too many columns to a table

It is better to limit the total no of columns in a table. Relational databases like mysql, can be leveraged to split the tables with so many columns into multiple tables. They can be joined together by using their primary and foreign keys.

Adding too many columns to a table will increase the individual record length and will slow down the table scan. When you are doing a select *  query, you will end up retrieving a bunch of columns which you really do not need.

## separate columns with text data type into their own table

> This tip is from personal experience and is not a standard way of architecting your database tables. I recommend to ollow this tip only if your table has too many records or will grow rapidly.

If a table has columns which stores large amounts of data(ex: columns with a datatype of TEXT), it is better to separate them into their own table or into a table which will be less frequently asked.

When the table has columns with large amounts of data in it, the size of an individual record grows really high. I ersonally observed it affected the query time on one of our projects.

Consider a case where you have a table called posts with a column of content which stores the blog post content.

The content for blog post will be really huge and often times, you need this data only if a person is viewing this particular blog post.

So separating this column from the posts table will drastically improve the query performance when there are too many posts.

## Better way to retrieve latest rows from a table

When we want to retrieve latest rows from a table, we would often do

```php
$posts = Post::latest()->get();
// or $posts = Post::orderBy('created_at', 'desc')->get();
```

The above approach will produce the following sql query.

```sql
select * from posts order by created_at desc
```

The query is basically ordering the rows in descending order based on the created_at column. Since created_at column is a string based column, it is often slower to order the results this way.

If your database table has an auto incrementing primary key id, then in most cases, the latest row will always have the highest id. Since id field is an integer field and also a primary key, it is much faster to order the results based on this key. So the better way to retrieve latest rows is as below.

```php
$posts = Post::latest('id')->get();
// or $posts = Post::orderBy('id', 'desc')->get();
```

```sql
select * from posts order by id desc
```

## optimize MySQL inserts

We so far looked into optimizing select queries for retrieving results from a database. Most cases we only need to optimize the read queries. But sometimes we find a need to optimize insert and update queries. I found an interesting article on optimizing mysql inserts which will helps in optimizling slow inserts and updates.

## Inspect and optimize queries

There is no one universal solution when optimizing queries in laravel. Only you know what your application is doing, how many queries it is making, how many of them are actually in use. So inspecting the queries made by your application will help you determine and reduce the total number of queries made.

There are certain tools which helps you in inspecting the queries made on each and every page.

> Note: It is recommended not to run any of these tools on your production environment. Running these on your production apps will degrade your application performance and when compromised, unauthorized users will get access to sensitive information.

#### 源自[laravel news - 18 Tips to Optimize Your Laravel Database Queries](https://laravel-news.com/18-tips-to-optimize-your-laravel-database-queries)