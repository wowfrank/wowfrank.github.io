---
layout: post
title: "Notes of Laravel Documentation"
date: 2020-01-18 00:00:00 +0800
description: "Notes of Laravel Documentation" # (optional)
img: Laravel-Logging.jpg # Add image post (optional)
fig-caption: "Notes of Laravel Documentation" # Add figcaption (optional)
tags: ['PHP','Laravel']
categories: ['Programming', 'PHP']
---

## Laravel Request

### Creating Form Requests

```bash
php artisan make:request StoreBlogPost
```

The generated class will be placed in the app/Http/Requests directory. If this directory does not exist, it will be created when you run the make:request command. Let's add a few validation rules to the rules method:

```php
/**
 * Get the validation rules that apply to the request.
 *
 * @return array
 */
public function rules()
{
    return [
        'title' => 'required|unique:posts|max:255',
        'body' => 'required',
    ];
}
```

### Authorizing Form Requests

The form request class also contains an authorize method. Within this method, you may check if the authenticated user actually has the authority to update a given resource. For example, you may determine if a user actually owns a blog comment they are attempting to update:

```php
/**
 * Determine if the user is authorized to make this request.
 *
 * @return bool
 */
public function authorize()
{
    $comment = Comment::find($this->route('comment'));

    return $comment && $this->user()->can('update', $comment);
}
```

### Customizing The Error Messages

You may customize the error messages used by the form request by overriding the messages method. This method should return an array of attribute / rule pairs and their corresponding error messages:

```php
/**
 * Get the error messages for the defined validation rules.
 *
 * @return array
 */
public function messages()
{
    return [
        'title.required' => 'A title is required',
        'body.required'  => 'A message is required',
    ];
}
```

### Customizing The Validation Attributes

If you would like the :attribute portion of your validation message to be replaced with a custom attribute name, you may specify the custom names by overriding the attributes method. This method should return an array of attribute / name pairs:

```php
/**
 * Get custom attributes for validator errors.
 *
 * @return array
 */
public function attributes()
{
    return [
        'email' => 'email address',
    ];
}
```

### Prepare Input For Validation

If you need to sanitize any data from the request before you apply your validation rules, you can use the prepareForValidation method:

```php
use Illuminate\Support\Str;

/**
 * Prepare the data for validation.
 *
 * @return void
 */
protected function prepareForValidation()
{
    $this->merge([
        'slug' => Str::slug($this->slug),
    ]);
}
```

## Laravel Log

Log::emergency($message);
Log::alert($message);
Log::critical($message);
Log::error($message);
Log::warning($message);
Log::notice($message);
Log::info($message);
Log::debug($message);

## Laravel Log Tutorials

[Laravel Logging Tutorial](https://stackify.com/laravel-logging-tutorial/){:target="_blank"}

[GETTING STARTED QUICKLY WITH LARAVEL LOGGING](https://www.scalyr.com/blog/getting-started-quickly-laravel-logging/){:target="_blank"}

[PHP Monolog Tutorial: A Step by Step Guide](https://stackify.com/php-monolog-tutorial/)