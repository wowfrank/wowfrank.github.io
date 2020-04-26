---
layout: post
title: "The Difference Between static and self in PHP (Notes) "
date: 2020-02-16 00:00:00 +0800
description: "The Difference Between static and self in PHP" # (optional)
img:  # Add image post (optional)
fig-caption: "" # Add figcaption (optional)
tags: ['static','self', 'PHP']
categories: ['Programming', 'PHP']
---

new static instantiates a new object from the current class, and works with **late static bindings** (instantiates the subclass if the class was subclassed, I expect you understand that).

> According to php.net, **late static bindings** work by storing the class named in the last "non-forwarding call". In case of static method calls, this is the class explicitly named (usually the one on the left of the :: operator); in case of non static method calls, it is the class of the object. A "forwarding call" is a static one that is introduced by self::, parent::, static::, or, if going up in the class hierarchy, forward_static_call(). The function get_called_class() can be used to retrieve a string with the name of the called class and static:: introduces its scope.

> This feature was named "late static bindings" with an internal perspective in mind. "Late binding" comes from the fact that static:: will not be resolved using the class where the method is defined but it will rather be computed using runtime information. It was also called a "static binding" as it can be used for (but is not limited to) static method calls.

Having a static method on a class which returns a new instance of same is an alternative constructor. Meaning, typically your constructor is public function \_\_construct, and typically it requires a certain bunch of parameters:

```php
<?php
	class Foo {
	    public function __construct(BarInterface $bar, array $baz = []) { ... }
	}
```

Having an alternative constructor allows you to provide different defaults, or convenience shortcuts to instantiate this class without having to supply those specific arguments and/or for being able to provide different arguments which the alternative constructor will convert to the canonical ones:

```php
<?php
	class Foo {

	    public function __construct(BarInterface $bar, array $baz = []) { ... }

	    public static function fromBarString($bar) {
	        return new static(new Bar($bar));
	    }

	    public static function getDefault() {
	        return new static(new Bar('baz'), [42]);
	    }

	}
```

Now, even though your canonical constructor requires a bunch of complex arguments, you can create a default instance of your class, which will probably be fine for most uses, simply with Foo::getDefault().

The canonical example in PHP for this is DateTime and DateTime::createFromFormat.

In your concrete example the alternative constructor doesn't actually do anything, so it's rather superfluous, but I expect that's because it's an incomplete example. If there's indeed an alternative constructor which does nothing other than new static, it's probably just meant as convenience syntax over (new Foo)->, which I find questionable.

```php
<?php
	class A {
	    public static function get_self() {
	        return new self();
	    }

	    public static function get_static() {
	        return new static();
	    }
	}

	class B extends A {}

	echo get_class(B::get_self());  // A
	echo get_class(B::get_static()); // B
	echo get_class(A::get_self()); // A
	echo get_class(A::get_static()); // A
```

---
---

## Late static binding in PHP - What, How & When

---
---

The inheritence tree in PHP quickly gets dirty if you’re using mix of static and non-static methods into your classes and the inheritence is deeply nested. Take from example here.

```php
<?php
	class Alpha 
	{
	    public static function getClassname() 
	    {
	        echo __CLASS__;
	    }

	    public static function printClass() 
	    {
	        self::getClassname();
	    }
	}

	class Beta extends Alpha 
	{
	    public static function getClassname() 
	    {
	        echo __CLASS__;
	    }
	}

	Beta::printClass();

	// Alpha
```

Ideally, if you run above code, you’d expect it to obviously print Beta but instead it’ll print Alpha. Why? Because static references to the current class like self:: or \_\_CLASS\_\_ are resolved using the class in which the function belongs or in other words self keyword does not follow the same rules of inheritance. self always resolves to the class in which it is used. If you want the desired result, you’ll need to use a feature called “Late static binding”.

### What is late static binding?

> Late static bindings in PHP is a feature which can be used to reference the called class in a context of static inheritance.

Essentially, late static bindings work by storing the class named in the last “non-forwarding call”. In case of static method calls, this is the class explicitly named (usually the one on the left of the :: operator); in case of non static method calls, it is the class of the object. A “forwarding call” is a static one that is introduced by self::, parent::, static::, or, if going up in the class hierarchy, forward_static_call(). The function get_called_class() can be used to retrieve a string with the name of the called class and static:: introduces its scope.

Let’s see how we can apply late static binding in the previous example and get our desired result.

```php
<?php
	class Alpha 
	{
	    public static function getClassname() 
	    {
	        echo __CLASS__;
	    }

	    public static function printClass() 
	    {
	        static::getClassname();
	    }
	}

	class Beta extends Alpha 
	{
	    public static function getClassname() 
	    {
	        echo __CLASS__;
	    }
	}

	Beta::printClass();

	// Beta
```

Running above PHP will now print Beta, because as you can see static::getClassname() will bring late static binding into the picture and now it will reference the class that was initially called at runtime which in our case is class Beta.

“Late binding” comes from the fact that static:: will not be resolved using the class where the method is defined but it will rather be computed using runtime information. It’s also called a “static binding” as it can be used for (but is not limited to) static method calls. Another important thing to note here is that static:: can only refer to static properties.

> **static** references calling classes scope.

Additionally, you can use get_called_class() method which returns the class on which the static method is called. Check it here.

```php
<?php
	class Alpha 
	{
	    public static function printClass() 
	    {
	        var_dump(get_called_class());
	    }
	}

	class Beta extends Alpha
	{

	}

	Beta::printClass(); // Beta
	Alpha::printClass(); // Alpha
```

### Late static binding on const

Late static binding would work the same way it is working for static methods. i.e. PHP will resolve the constant based on the fact that how it is been called.

```php
<?php
	class Alpha 
	{
	    const MY_CONST = false;

	    public function selfConst() 
	    {
	        return self::MY_CONST;
	    } 

	    public function staticConst() 
	    {
	        return static::MY_CONST;
	    } 
	}

	class Beta extends Alpha 
	{
	   const MY_CONST = true;
	}

	$beta = new Beta();
	echo $beta->selfConst() ? 'yes' : 'no'; // prints "no"
	echo $beta->staticConst() ? 'yes' : 'no'; // prints "yes"
```

In order for late static binding to work on constant, it need not necessarily be static unlike methods as you can see in the example above.

### When to use late static binding

Late static binding can be used for the following scenarios:

- When you’ve a functionality in your application that varies over the class hierarchy.
- When the methods in which the functionality is implemented has the same signature over the hierarchy.
- It can be used to create static factory patterns using static method overloading to prevent requiring additional factory classes.