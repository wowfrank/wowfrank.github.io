---
layout: post
title: "Working with JSON in MYSQL"
date: 2021-03-07 00:07:00 +0800
description: "Working with JSON in MYSQL" # (optional)
img: 2021-03-07-cover-image-of-working-with-json-in-mysql.png # Add image post (optional)
fig-caption: "Working with JSON in MYSQL" # Add figcaption (optional)
tags: ['Programming', 'JSON']
categories: ['Programming', 'JSON']
---

## Introduction

MySQL version 5.7.8 introduces a JSON data type that allows you to access data in JSON documents.

SQL databases tend to be rigid in design. By its nature, the structured query language enforces data type and size constraints.

In comparison, NoSQL databases encourage flexibility in design. In these schema-less databases, there is no imposed structural restriction, only data to be saved.

The JSON data type in MySQL grants you the strengths of both of these systems. It allows you to structure some parts of your database and leave others to be flexible.

The first half of this article will design a database with JSON fields. It will step through using the built-in functions available to MySQL to create, read, update, and delete rows.

The second half of this article will utilize the Eloquent ORM with Laravel to communicate with the database. You will build an admin panel that supports displaying products, adding new products, modifying existing products, and deleting products.

## Prerequisites

If you would like to follow along with this article, you will need:

- MySQL 5.7.8 or later and PHP 7.3.24 or later. You can consult our tutorials on installing Linux, Apache, MySQL, and PHP

- Some familiarity with SQL queries.

- Some familiarity with writing PHP.

- Some familiarity with Laravel.

- This tutorial utilizes Laravel installation via Composer in mind. You can consult our tutorial on installing Composer.

Note: Laravel now provides a tool called Sail to work with Docker that will configure an environment with MySQL, PHP, and Composer.

This may be an alternative option if you are having difficulty setting up your local environment.

This tutorial was verified with MySQL v8.0.23, PHP v7.3.24, Composer v2.0.9, and Laravel v8.26.1.

## Defining the Schema

For the purposes of this tutorial, you will be building from a schema that defines the inventory of an online store that sells a variety of electronics.

Traditionally, the Entity–attribute–value model (EAV) pattern would be used to allow customers to compare the features of products.

However, with the JSON data type, this use case can be approached differently.

The database will be named ```e_store``` and have three tables named ```brands```, ```categories``` and ```products``` respectively.

Create the e_store database:

```sql
CREATE DATABASE IF NOT EXISTS `e_store`
DEFAULT CHARACTER SET utf8
DEFAULT COLLATE utf8_general_ci;

SET default_storage_engine = INNODB;
```
 
The ```brands``` and ```categories``` tables will each have an ```id``` and a ```name``` field.

Create the brands table:

```sql
CREATE TABLE `e_store`.`brands`(
    `id` INT UNSIGNED NOT NULL auto_increment ,
    `name` VARCHAR(250) NOT NULL ,
    PRIMARY KEY(`id`)
);
```

Create the categories table:

```sql
CREATE TABLE `e_store`.`categories`(
    `id` INT UNSIGNED NOT NULL auto_increment ,
    `name` VARCHAR(250) NOT NULL ,
    PRIMARY KEY(`id`)
);
```

Next, add some sample brands:

```sql
INSERT INTO `e_store`.`brands`(`name`)
VALUES
    ('Samsung');

INSERT INTO `e_store`.`brands`(`name`)
VALUES
    ('Nokia');

INSERT INTO `e_store`.`brands`(`name`)
VALUES
    ('Canon');
```

Then, add some categories:

```sql
INSERT INTO `e_store`.`categories`(`name`)
VALUES
    ('Television');

INSERT INTO `e_store`.`categories`(`name`)
VALUES
    ('Mobile Phone');

INSERT INTO `e_store`.`categories`(`name`)
VALUES
    ('Camera');
```

Next, create a products table with the id, name, brand_id, category_id, and attributes fields:

```sql
CREATE TABLE `e_store`.`products`(
    `id` INT UNSIGNED NOT NULL AUTO_INCREMENT ,
    `name` VARCHAR(250) NOT NULL ,
    `brand_id` INT UNSIGNED NOT NULL ,
    `category_id` INT UNSIGNED NOT NULL ,
    `attributes` JSON NOT NULL ,
    PRIMARY KEY(`id`) ,
    INDEX `CATEGORY_ID`(`category_id` ASC) ,
    INDEX `BRAND_ID`(`brand_id` ASC) ,
    CONSTRAINT `brand_id` FOREIGN KEY(`brand_id`) REFERENCES `e_store`.`brands`(`id`) ON DELETE RESTRICT ON UPDATE CASCADE ,
    CONSTRAINT `category_id` FOREIGN KEY(`category_id`) REFERENCES `e_store`.`categories`(`id`) ON DELETE RESTRICT ON UPDATE CASCADE
);
```

This table definition specifies foreign key constraints for the ```brand_id``` and ```category_id``` fields, specifying that they reference the ```brands``` and ```categories``` table respectively. This table definition also specifies that the referenced rows should not be allowed to delete and if updated, the changes should reflect in the references as well.

The ```attributes``` field’s column type has been declared to be JSON which is the native data type now available in MySQL. This allows you to use the various JSON related constructs in MySQL on the ```attributes``` field.

Here is an entity relationship diagram of the created database:

![entity relationship diagram]({{site.baseurl}}/assets/img/2021-03-07/2021-03-07-diagram-of-database-relationship.jpeg)

An entity relationship diagram of the e_store database

This database design is not the best in terms of efficiency and accuracy. There are some common real-world use cases that are not accounted for. For example, there is no price column in the products table and there is no support for a product being in multiple categories. However, the purpose of this tutorial is not to teach database design but rather how to model objects of different nature in a single table using MySQL’s JSON features.

## Creating Data in the JSON Field

Now, you are going to create products to add to the database using ```INSERT INTO``` and ```VALUES```.

Here are some example televisions with data on screen size, resolution, ports, and speakers using stringified JSON objects:

```sql
INSERT INTO `e_store`.`products`(
    `name` ,
    `brand_id` ,
    `category_id` ,
    `attributes`
)
VALUES(
    'Prime' ,
    '1' ,
    '1' ,
    '{"screen": "50 inch", "resolution": "2048 x 1152 pixels", "ports": {"hdmi": 1, "usb": 3}, "speakers": {"left": "10 watt", "right": "10 watt"}}'
);

INSERT INTO `e_store`.`products`(
    `name` ,
    `brand_id` ,
    `category_id` ,
    `attributes`
)
VALUES(
    'Octoview' ,
    '1' ,
    '1' ,
    '{"screen": "40 inch", "resolution": "1920 x 1080 pixels", "ports": {"hdmi": 1, "usb": 2}, "speakers": {"left": "10 watt", "right": "10 watt"}}'
);

INSERT INTO `e_store`.`products`(
    `name` ,
    `brand_id` ,
    `category_id` ,
    `attributes`
)
VALUES(
    'Dreamer' ,
    '1' ,
    '1' ,
    '{"screen": "30 inch", "resolution": "1600 x 900 pixles", "ports": {"hdmi": 1, "usb": 1}, "speakers": {"left": "10 watt", "right": "10 watt"}}'
);

INSERT INTO `e_store`.`products`(
    `name` ,
    `brand_id` ,
    `category_id` ,
    `attributes`
)
VALUES(
    'Bravia' ,
    '1' ,
    '1' ,
    '{"screen": "25 inch", "resolution": "1366 x 768 pixels", "ports": {"hdmi": 1, "usb": 0}, "speakers": {"left": "5 watt", "right": "5 watt"}}'
);

INSERT INTO `e_store`.`products`(
    `name` ,
    `brand_id` ,
    `category_id` ,
    `attributes`
)
VALUES(
    'Proton' ,
    '1' ,
    '1' ,
    '{"screen": "20 inch", "resolution": "1280 x 720 pixels", "ports": {"hdmi": 0, "usb": 0}, "speakers": {"left": "5 watt", "right": "5 watt"}}'
);
```

This example declares five different television products.

Alternatively, you could use the built-in ```JSON_OBJECT``` function to create JSON objects.

The ```JSON_OBJECT``` function accepts a list of key/value pairs in the form JSON_OBJECT(key1, value1, key2, value2, ... key(n), value(n)) and returns a JSON object.

Here are some example mobile phones using the ```JSON_OBJECT``` function:

```sql
INSERT INTO `e_store`.`products`(
    `name` ,
    `brand_id` ,
    `category_id` ,
    `attributes`
)
VALUES(
    'Desire' ,
    '2' ,
    '2' ,
    JSON_OBJECT(
        "network" ,
        JSON_ARRAY("GSM" , "CDMA" , "HSPA" , "EVDO") ,
        "body" ,
        "5.11 x 2.59 x 0.46 inches" ,
        "weight" ,
        "143 grams" ,
        "sim" ,
        "Micro-SIM" ,
        "display" ,
        "4.5 inches" ,
        "resolution" ,
        "720 x 1280 pixels" ,
        "os" ,
        "Android Jellybean v4.3"
    )
);

INSERT INTO `e_store`.`products`(
    `name` ,
    `brand_id` ,
    `category_id` ,
    `attributes`
)
VALUES(
    'Passion' ,
    '2' ,
    '2' ,
    JSON_OBJECT(
        "network" ,
        JSON_ARRAY("GSM" , "CDMA" , "HSPA") ,
        "body" ,
        "6.11 x 3.59 x 0.46 inches" ,
        "weight" ,
        "145 grams" ,
        "sim" ,
        "Micro-SIM" ,
        "display" ,
        "4.5 inches" ,
        "resolution" ,
        "720 x 1280 pixels" ,
        "os" ,
        "Android Jellybean v4.3"
    )
);

INSERT INTO `e_store`.`products`(
    `name` ,
    `brand_id` ,
    `category_id` ,
    `attributes`
)
VALUES(
    'Emotion' ,
    '2' ,
    '2' ,
    JSON_OBJECT(
        "network" ,
        JSON_ARRAY("GSM" , "CDMA" , "EVDO") ,
        "body" ,
        "5.50 x 2.50 x 0.50 inches" ,
        "weight" ,
        "125 grams" ,
        "sim" ,
        "Micro-SIM" ,
        "display" ,
        "5.00 inches" ,
        "resolution" ,
        "720 x 1280 pixels" ,
        "os" ,
        "Android KitKat v4.3"
    )
);

INSERT INTO `e_store`.`products`(
    `name` ,
    `brand_id` ,
    `category_id` ,
    `attributes`
)
VALUES(
    'Sensation' ,
    '2' ,
    '2' ,
    JSON_OBJECT(
        "network" ,
        JSON_ARRAY("GSM" , "HSPA" , "EVDO") ,
        "body" ,
        "4.00 x 2.00 x 0.75 inches" ,
        "weight" ,
        "150 grams" ,
        "sim" ,
        "Micro-SIM" ,
        "display" ,
        "3.5 inches" ,
        "resolution" ,
        "720 x 1280 pixels" ,
        "os" ,
        "Android Lollipop v4.3"
    )
);

INSERT INTO `e_store`.`products`(
    `name` ,
    `brand_id` ,
    `category_id` ,
    `attributes`
)
VALUES(
    'Joy' ,
    '2' ,
    '2' ,
    JSON_OBJECT(
        "network" ,
        JSON_ARRAY("CDMA" , "HSPA" , "EVDO") ,
        "body" ,
        "7.00 x 3.50 x 0.25 inches" ,
        "weight" ,
        "250 grams" ,
        "sim" ,
        "Micro-SIM" ,
        "display" ,
        "6.5 inches" ,
        "resolution" ,
        "1920 x 1080 pixels" ,
        "os" ,
        "Android Marshmallow v4.3"
    )
);
```

This example declares five different mobile phone products.

It also utilized the ```JSON_ARRAY``` function which returns a JSON array when passed a set of values.

If you specify a single key multiple times, only the first key/value pair will be retained. This is called normalizing the JSON in MySQL’s terms. Also, as part of normalization, the object keys are sorted and the extra white-space between key/value pairs is removed.

Additionally, you could use the built-in ```JSON_MERGE_PRESERVE``` or ```JSON_MERGE_PATCH``` functions to create JSON objects.

Note: In previous versions of MySQL, you could use ```JSON_MERGE```, but this function has been deprecated.

'JSON_MERGE' is deprecated and will be removed in a future release. Please use JSON_MERGE_PRESERVE/JSON_MERGE_PATCH instead

For the purpose of this tutorial, you will use the ```JSON_MERGE_PRESERVE``` function. This function takes multiple JSON objects and produces a single, aggregate object.

Here are some example cameras using the ```JSON_MERGE_PRESERVE``` function:

```sql
INSERT INTO `e_store`.`products`(
    `name` ,
    `brand_id` ,
    `category_id` ,
    `attributes`
)
VALUES(
    'Explorer' ,
    '3' ,
    '3' ,
    JSON_MERGE_PRESERVE(
        '{"sensor_type": "CMOS"}' ,
        '{"processor": "Digic DV III"}' ,
        '{"scanning_system": "progressive"}' ,
        '{"mount_type": "PL"}' ,
        '{"monitor_type": "LCD"}'
    )
);

INSERT INTO `e_store`.`products`(
    `name` ,
    `brand_id` ,
    `category_id` ,
    `attributes`
)
VALUES(
    'Runner' ,
    '3' ,
    '3' ,
    JSON_MERGE_PRESERVE(
        JSON_OBJECT("sensor_type" , "CMOS") ,
        JSON_OBJECT("processor" , "Digic DV II") ,
        JSON_OBJECT("scanning_system" , "progressive") ,
        JSON_OBJECT("mount_type" , "PL") ,
        JSON_OBJECT("monitor_type" , "LED")
    )
);

INSERT INTO `e_store`.`products`(
    `name` ,
    `brand_id` ,
    `category_id` ,
    `attributes`
)
VALUES(
    'Traveler' ,
    '3' ,
    '3' ,
    JSON_MERGE_PRESERVE(
        JSON_OBJECT("sensor_type" , "CMOS") ,
        '{"processor": "Digic DV II"}' ,
        '{"scanning_system": "progressive"}' ,
        '{"mount_type": "PL"}' ,
        '{"monitor_type": "LCD"}'
    )
);

INSERT INTO `e_store`.`products`(
    `name` ,
    `brand_id` ,
    `category_id` ,
    `attributes`
)
VALUES(
    'Walker' ,
    '3' ,
    '3' ,
    JSON_MERGE_PRESERVE(
        '{"sensor_type": "CMOS"}' ,
        '{"processor": "Digic DV I"}' ,
        '{"scanning_system": "progressive"}' ,
        '{"mount_type": "PL"}' ,
        '{"monitor_type": "LED"}'
    )
);

INSERT INTO `e_store`.`products`(
    `name` ,
    `brand_id` ,
    `category_id` ,
    `attributes`
)
VALUES(
    'Jumper' ,
    '3' ,
    '3' ,
    JSON_MERGE_PRESERVE(
        '{"sensor_type": "CMOS"}' ,
        '{"processor": "Digic DV I"}' ,
        '{"scanning_system": "progressive"}' ,
        '{"mount_type": "PL"}' ,
        '{"monitor_type": "LCD"}'
    )
);
```

This example declares five different camera products.

Notice that only objects are passed to the ```JSON_MERGE_PRESERVE``` function. Some of them have been constructed using the ```JSON_OBJECT``` function. Others have been passed as valid JSON strings.

In the case of the ```JSON_MERGE_PRESERVE``` function, if a key is repeated multiple times, its value is retained as an array in the output.

For example, here is a collection of JSON objects with the same ```network``` key:

```sql
SELECT JSON_MERGE_PRESERVE(
    '{"network": "GSM"}' ,
    '{"network": "CDMA"}' ,
    '{"network": "HSPA"}' ,
    '{"network": "EVDO"}'
);
```

This will produce an array of values:

```
Output
{"network": ["GSM", "CDMA", "HSPA", "EVDO"]}
```

Now, at this point, you can verify your queries by using the JSON_TYPE function to display the field value type:

```sql
SELECT JSON_TYPE(attributes) FROM `e_store`.`products`;
```

This query will produce 15 ```OBJECT``` results to represent all of the products - five televisions, five mobile phones, and five cameras.

Now, you can create data in the JSON field.

## Reading the Data from the JSON Field

Now that you have some products in the database to work with, you can experiment with reading the data.

For typical MySQL values that are not of type JSON, you would usually rely upon a WHERE clause. Heuristically, when working with JSON columns, this does not work.

When you wish to select rows using a JSON field, you should be familiar with the concept of a path expression. Path expressions use a dollar sign symbol ($) and the target object keys.

When used in combination with the ```JSON_EXTRACT``` function, you can retrieve the values for the specified column.

Consider a scenario where you are interested in all of the televisions that have at least one USB and one HDMI port:

```sql
SELECT
    *
FROM
    `e_store`.`products`
WHERE
    `category_id` = 1
AND JSON_EXTRACT(`attributes` , '$.ports.usb') > 0
AND JSON_EXTRACT(`attributes` , '$.ports.hdmi') > 0;
```

The first argument to the ```JSON_EXTRACT``` function is the JSON to apply the path expression to which is the ```attributes``` column. The ```$``` symbol tokenizes the object to work with. The ```$.ports.usb``` and ```$.ports.hdmi``` path expressions translate to “take the usb key under ports” and “take the hdmi key under ports” respectively.

Once you have extracted the keys you are interested in, you can use the MySQL operators such as the greater than symbol (>) on them.

This query will produce three results:

![query result]({{site.baseurl}}/assets/img/2021-03-07/2021-03-07-query-result-01.png)

Screenshot of query results displaying the rows for Prime, Octoview, and Dreamer models of televisions

These three televisions have at least one USB port and one HDMI port. The “Bravia” and “Proton” models do not meet these conditions.

Alternatively, the ```JSON_EXTRACT``` function has the alias -> that you can use to make your queries more readable.

Revise the previous query to use the ```->``` alias:

```sql
SELECT
    *
FROM
    `e_store`.`products`
WHERE
    `category_id` = 1
AND `attributes` -> '$.ports.usb' > 0
AND `attributes` -> '$.ports.hdmi' > 0;
```

Now, you can read data from the JSON field.

## Updating Data in the JSON Field

You can update data in JSON fields with the ```JSON_INSERT```, ```JSON_REPLACE```, and ```JSON_SET``` functions. These functions also require a path expression to specify which parts of the JSON object to modify. The output of these functions is a valid JSON object with the changes applied.

First, update JSON fields with ```JSON_INSERT``` to add a new ```chipset``` key with the value “Qualcomm” for all mobile phones:

```sql
UPDATE `e_store`.`products`
SET `attributes` = JSON_INSERT(
    `attributes` ,
    '$.chipset' ,
    'Qualcomm'
)
WHERE
    `category_id` = 2;
```

The $.chipset path expression identifies the position of the ```chipset``` property to be at the root of the object.

Examine the updated mobile phone category with the following query:

```sql
SELECT
    *
FROM
    `e_store`.`products`
WHERE
    `category_id` = 2
```

“Qualcomm” is now present for all mobile phones:

![query result]({{site.baseurl}}/assets/img/2021-03-07/2021-03-07-query-result-02.png)

Screenshot of query results displaying the rows for models of mobile phones with the chipset of Qualcomm now added.

Now, update JSON fields with JSON_REPLACE to modify the existing chipset key with the value “Qualcomm Snapsdragon” for all mobile phones:

```sql
UPDATE `e_store`.`products`
SET `attributes` = JSON_REPLACE(
    `attributes` ,
    '$.chipset' ,
    'Qualcomm Snapdragon'
)
WHERE
    `category_id` = 2;
```

“Qualcomm” is now replaced with “Qualcomm Snapdragon” for all mobile phones:

![query result]({{site.baseurl}}/assets/img/2021-03-07/2021-03-07-query-result-03.png)

Screenshot of query results displaying the rows for models of mobile phones with the chipset of Qualcomm now changed to Qualcomm Snapdragon.

Lastly, update JSON fields with ```JSON_SET``` to add a new ```body_color``` key with the value “red” for all televisions:

```sql
UPDATE `e_store`.`products`
SET `attributes` = JSON_SET(
    `attributes` ,
    '$.body_color' ,
    'red'
)
WHERE
    `category_id` = 1;
```

“red” color is now applied to all televisions:

![query result]({{site.baseurl}}/assets/img/2021-03-07/2021-03-07-query-result-04.png)

Screenshot of query results displaying the rows for models of televisions with the color of red now added.

All of these functions seem identical but there is a difference in the way they behave.

- The ```JSON_INSERT``` function will only add the property to the object if it does not exists already.

- The ```JSON_REPLACE``` function substitutes the property only if it is found.

- The ```JSON_SET``` function will add the property if it is not found else replace it.

Now, you can update data from the JSON field.

## Deleting Data from the JSON Field

You can delete data in JSON fields with the ```JSON_REMOVE``` function and ```DELETE```.

```JSON_REMOVE``` allows you to delete a certain key/value from your JSON columns.

Using ```JSON_REMOVE``` function, it is possible to remove the ```mount_type``` key/value pairs from all cameras:

```sql
UPDATE `e_store`.`products`
SET `attributes` = JSON_REMOVE(`attributes` , '$.mount_type')
WHERE
    `category_id` = 3;
```

The ```JSON_REMOVE``` function returns the updated JSON after removing the specified key based on the path expression.

Alternatively, you can ```DELETE``` entire rows using a JSON column.

Using ```DELETE``` and ```JSON_EXTRACT``` and ```LIKE```, it is possible to remove all the mobile phones that have the “Jellybean” version of the Android operating system:

```sql
DELETE FROM `e_store`.`products`
WHERE `category_id` = 2
AND JSON_EXTRACT(`attributes` , '$.os') LIKE '%Jellybean%';
```

This query will remove the “Desire” and “Passion” models of mobile phones.

Working with a specific attribute requires the use of the ```JSON_EXTRACT``` function. First, the ```os``` property of mobile phones is extracted. And then the LIKE operator is applied to ```DELETE``` all records that contain the string ```Jellybean```.

Now, you can delete data from the JSON field.

## Creating the Migrations

Now, create a new Laravel project.

Warning: This web application is for tutorial purposes only and should not be used in a production setting.

Open your terminal window and run the following command:

```sh
$ composer create-project laravel/laravel estore-example
```

Navigate to the newly created project directory:

```sh
$ cd estore-example
```

Configure your Laravel application to use a MySQL database.

You may need to modify your ```.env``` file to set the ```DB_DATABASE```, ```DB_USERNAME```, and ```DB_PASSWORD```.

You are going to create three migrations for ```brands```, ```categories```, and ```products``` respectively.

Make a ```create_brands``` migration:

```sh
$ php artisan make:migration create_brands
```

Modify the ```create_brands.php``` migration with the following lines of code:

```php
<?php
// database/migrations/(…)create_brands.php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateBrands extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('brands', function(Blueprint $table){
            $table->engine = 'InnoDB';
            $table->increments('id');
            $table->string('name');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('brands');
    }
}
```

Make a ```create_categories``` migration:

```sh
$ php artisan make:migration create_categories
```
 
Modify the ```create_categories.php``` migration with the following lines of code:

```php
<?php
// database/migrations/(…)create_categories.php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateCategories extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('categories', function(Blueprint $table){
            $table->engine = 'InnoDB';
            $table->increments('id');
            $table->string('name');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('categories');
    }
}
```

The ```create_products``` migration will also have the directives for indexes and foreign keys:

```sh
$ php artisan make:migration create_products
```
 
Modify the ```create_products.php``` migration with the following lines of code:

```php
<?php
// database/migrations/(…)create_products.php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateProducts extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('products', function(Blueprint $table){
            $table->engine = 'InnoDB';
            $table->increments('id');
            $table->string('name');
            $table->unsignedInteger('brand_id');
            $table->unsignedInteger('category_id');
            $table->json('attributes');
            $table->timestamps();
            // foreign key constraints
            $table->foreign('brand_id')->references('id')->on('brands')->onDelete('restrict')->onUpdate('cascade');
            $table->foreign('category_id')->references('id')->on('categories')->onDelete('restrict')->onUpdate('cascade');
            // indexes
            $table->index('brand_id');
            $table->index('category_id');
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('products');
    }
}
```

Pay attention to the $table->json('attributes'); statement in the migration.

Note: This will only work for database engines that support the JSON data type.

Engines, such as older versions of MySQL will not be able to carry out these migrations.

Similar to creating other types of table fields using the appropriate data type named method, you have created a JSON column using the json method with the name attributes.

## Creating the Models

You are going to create three models for ```brands```, ```categories```, and ```products``` respectively.

Create a ```Brand``` model:

php artisan make:model Brand
 
Modify the ```Brand.php``` file with the following lines of code:

```php
<?php
// app/Models/Brand.php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Brand extends Model
{
    use HasFactory;

    // A brand has many products
    public function products(){
        return $this->hasMany('Product')
    }
}
```

Create a ```Category``` model:

```sh
$ php artisan make:model Category
```
 
Modify the ```Category.php``` file with the following lines of code:

```php
<?php
// app/Models/Category.php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Category extends Model
{
    // A category has many products
    public function products(){
        return $this->hasMany('Product')
    }
}
```

Create a ```Product``` model:

```sh
$ php artisan make:model Product
```
 
Modify the ```Product.php``` file with the following lines of code:

```php
<?php
// app/Models/Product.php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Product extends Model
{
    use HasFactory;

    public $timestamps = false;

    // Cast attributes JSON to array
    protected $casts = [
        'attributes' => 'array'
    ];

    // Each product has a brand
    public function brand(){
        return $this->belongsTo('Brand');
    }

    // Each product has a category
    public function category(){
        return $this->belongsTo('Category');
    }
}
```

The ```$casts``` array which has the key ```attributes``` set to array makes sure whenever a product is fetched from the database, its ```attributes``` JSON is converted to an associated array. This allows you to update records from your controller actions.

## Creating a Product

The focus of the remainder of this tutorial will be on the camera product category.

You will be building a view with a form that has fields that are specific for cameras. For brevity, the television and mobile phone product categories will not be covered - but would be very similar in design.

Create the controller for the camera product category:

```sh
$ php artisan make:controller CameraController
```

Modify the ```CameraController.php``` with the following lines of code:

```php
<?php
// app/Http/Controller/CameraController.php
namespace App\Http\Controllers;

use Illuminate\Http\Request;

class CameraController extends Controller
{
    // creates product in database
    // using form fields
    public function store(Request $request){
        // create object and set properties
        $camera = new \App\Models\Product();
        $camera->name = $request->name;
        $camera->brand_id = $request->brand_id;
        $camera->category_id = $request->category_id;
        $camera->attributes = [
            'processor' => $request->processor,
            'sensor_type' => $request->sensor_type,
            'monitor_type' => $request->monitor_type,
            'scanning_system' => $request->scanning_system,
        ];
        // save to database
        $camera->save();
        // show the created camera
        return view('product.camera.show', ['camera' => $camera]);
    }
}
```

This completes the ```store``` function for cameras.

Create a view by making a ```new.blade.php``` file in the ```resources/views/product/camera``` directory tree:

```php
<form method="POST" action="/product/camera/store">
    @csrf
    <table>
        <tr>
            <td><label for="name">Name</label></td>
            <td><input id="name" name="name" type="text"></td>
        </tr>
        <tr>
            <td><label for="brand-id">Brand ID</label></td>
            <td>
                <select id="brand-id" name="brand_id">
                    <option value="1">Samsung</option>
                    <option value="2">Nokia</option>
                    <option value="3">Canon</option>
                </select>
            </td>
        </tr>
        <tr>
            <td><label for="attributes-processor">Processor</label></td>
            <td><input id="attributes-processor" name="processor" type="text"></td>
        </tr>
        <tr>
            <td><label for="attributes-sensor-type">Sensor Type</label></td>
            <td><input id="attributes-sensor-type" name="sensor_type" type="text"></td>
        </tr>
        <tr>
            <td><label for="attributes-monitor-type">Monitor Type</label></td>
            <td><input id="attributes-monitor-type" name="monitor_type" type="text"></td>
        </tr>
        <tr>
            <td><label for="attributes-scanning-system">Scanning System</label></td>
            <td><input id="attributes-scanning-system" name="scanning_system" type="text"></td>
        </tr>
    </table>
    <input name="category_id" value="3" type="hidden">
    <button type="submit">Submit</button>
</form>
```

The ```brand_id``` is presented as a hardcoded ```select``` element with the three brands that were created earlier as options. The ```category_id``` is presented as a hardcoded hidden input value set to the id for cameras.

Modify the routes in ```routes/web.php``` to display the cameras:

```php
// routes/web.php

use App\Http\Controllers\CameraController;

Route::get('/product/camera/new', function() {
    return view('product/camera/new');
});

Route::post(
    '/product/camera/store',
    [CameraController::class, 'store']
);
```

Serve the application with the following command:

```sh
$ php artisan serve
```
 
Then, visit localhost:8000/product/camera/new) with your web browser. It will display a form for adding a new camera.

## Fetching Products

The ```$casts``` array that was declared earlier in the Product model will help you read and edit a product by treating attributes as an associative array.

Modify the ```CamerasController``` with the following lines of code:

```php
<?php

// ...app/Http/Controller/CameraController.php

class CameraController extends Controller
{
    // ... store ...

    // fetches a single product
    // from database
    public function show($id){
        $camera = \App\Models\Product::find($id);
        return view('product.camera.show', ['camera' => $camera]);
    }
}
```

This completes the ```show``` function for cameras.

Create a view by making a ```show.blade.php``` file in the ```resources/views/product/camera``` directory tree:

```php
<table>
    <tr>
        <td>Name</td>
        <td>{{ $camera->name }}</td>
    </tr>
    <tr>
        <td>Brand ID</td>
        <td>{{ $camera->brand_id }}</td>
    </tr>
    <tr>
        <td>Category ID</td>
        <td>{{ $camera->category_id }}</td>
    </tr>
    <tr>
        <td>Processor</td>
        <td>{{ $camera->attributes['processor'] }}</td>
    </tr>
    <tr>
        <td>Sensor Type</td>
        <td>{{ $camera->attributes['sensor_type'] }}</td>
    </tr>
    <tr>
        <td>Monitor Type</td>
        <td>{{ $camera->attributes['monitor_type'] }}</td>
    </tr>
    <tr>
        <td>Scanning System</td>
        <td>{{ $camera->attributes['scanning_system'] }}</td>
    </tr>
</table>
```

Modify the routes in routes/web.php to display the cameras:

```php
// ...routes/web.php

Route::get(
    '/product/camera/show/{id}',
    [CameraController::class, 'show']
);
```

Serve the application with the following command:

```sh
$ php artisan serve
```
 
Then, visit a valid id for a camera product (e.g., localhost:8000/product/camera/show/11) with your web browser. It will display a table of the camera information for the product with an id of “11”.

## Editing a Product

By using a combination of the techniques for store and show, you can create a view to edit an existing product.

You can create a form similar to the one in new.blade.php. And then prepopulate it with a product variable similiar to the one used in show.blade.php:

```php
<tr>
    <td><label for="attributes-processor">Processor</label></td>
    <td><input id="attributes-processor" name="processor" type="text" value="{{ $camera->attributes['processor'] }}"></td>
</tr>
```

Now, the form displays the existing values, making it easier for users to see what needs updating.

First, the id is used to retrieve the model. Next, the values from the request are applied. Lastly, the new values are saved to the database.

## Searching Based on JSON Attributes

You can also query JSON columns using the Eloquent ORM.

Consider a search page that allows users to search for cameras based upon attributes that they are interested in.

```php
public function search(Request $request){
    $cameras = \App\Models\Product::where([
        ['attributes->processor', 'like', $request->processor],
        ['attributes->sensor_type', 'like', $request->sensor_type],
        ['attributes->monitor_type', 'like', $request->monitor_type],
        ['attributes->scanning_system', 'like', $request->scanning_system]
    ])->get();
    return view('product.camera.search', ['cameras' => $cameras]);
}
```

The retrieved records will now be available to the product.camera.search view as a $cameras collection. This will allow you to loop through the results and display the cameras that satisfy the conditions from the user’s search request.

## Deleting a Product

Using a non-JSON column attribute, you can delete products by specifying a where clause and then calling the delete method.

For example, in the case of an id.

```php
\App\Models\Product::where('id', $id)->delete();
```

For JSON columns, specify a where clause using a single or multiple attributes and then call the delete method.

```php
\App\Models\Product::where('attributes->sensor_type', 'CMOS')->delete();
```
 
In this example, this code will remove all products that have a sensor_type attribute set to “CMOS”.

## Conclusion

In this article, you designed a MySQL database with the JSON data type and connected to it with a Laravel web application.

Whenever you need to save data as key/value pairs in a separate table or work with flexible attributes for an entity, you should consider using a JSON data type field instead as it can heavily contribute to compressing your database design.