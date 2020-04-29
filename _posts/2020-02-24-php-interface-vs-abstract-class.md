---
layout: post
title: "PHP Differences Between Interface And Abstract Class"
date: 2020-02-24 00:00:00 +0800
description: "PHP Differences Between Interface And Abstract Class" # (optional)
img: vs-abstract-class.jpg # Add image post (optional)
fig-caption: "PHP Differences Between Interface And Abstract Class" # Add figcaption (optional)
tags: ['PHP', 'Interface', 'Abstract']
categories: ['PHP', 'Programming']
---

PHP is among the most popular web development languages and widely used for website and web application development. Since the release of PHP 5 in 2004, OOP was introduced in PHP and it continuously evolved to the present where it's very easy to develop software in PHP by applying OOPs concepts. The most recent versions of PHP since PHP 7.0 have focused a lot on supporting types including return type declaration. Interfaces and Classes are the most important aspect of any OOP language and considered as the base for designing good software that can be easily understood, extended and managed by others. This tutorial explains the differences between an Interface and Abstract Class with examples.

We can use both the Interfaces and Abstract classes for abstraction to leave the actual implementation by just designing the prototype. The actual implementation can be done by the classes implementing the interfaces and the abstract classes. An Interface or Abstract Class cannot be considered as fully implemented until all the abstract functions are defined.

Notes: This tutorial assumes that you are using PHP 7.1 or above and already familiar with using Composer. You can also follow How To Autoload PHP Classes Using Composer to set up your project and it also shows using classes and interfaces with real example

## Interface Examples

An Interface can be considered as the prototype or blueprint to be implemented by the classes. The interfaces can only declare functions without any definition part as shown below. I have covered the important concepts of Interfaces in these examples.

```php
// An Interface without functions, using constants
interface A {

	// Constant
	const GLOBAL_A = 10;
}

// An Interface with constructor and public functions declaration
interface B {

	// Constant
	const GLOBAL_B = 20;

	// Contructor
	public function __construct();

	// Public Function - Add
	public function add( int $a, int $b ): int;

	// Public Function - Sub
	abstract function sub( int $a, int $b ): int;
}

// An Interface showing single inheritance
interface C extends A {

	// Public Function - Add
	public function add( int $a, int $b ): int;
}

// An Interface showing multiple inheritance
interface D extends A, B {

	// Public Function - Multiple
	public function multiply( int $a, int $b ): int;

	// Public Function - Division
	public function division( int $a, int $b ): float;
}
```

## Abstract Class Examples

A class can be considered as an Abstract Class if it contains the abstract keyword in its declaration. We cannot create objects of Abstract Classes. The Abstract Class can declare abstract methods, but a concrete class without the abstract keyword in its declaration cannot declare abstract methods. The below examples show the abstract classes.

```php
// An Abstract Class without methods
abstract class CA implements A {

	// Constant
	const GLOBAL_C = 30;

	// Static Variable
	public static $d = 60;

	// Public Member Variable
	public $e = 70;

	// Protected Member Variable
	protected $f = 80;

	// Private Member Variable
	private $g = 90;
}

// An Abstract Class with abstract and non abstract methods
abstract class CB implements B {

	public function __construct() {

		echo "In CB constructor.";
	}

	// Abstract Method with public and abstract modifiers
	public abstract function add( int $a, int $b ) : int;

	// Public Method
	public function sub( int $a, int $b ) : int {

		return $a - $b;
	}
}

// An Abstract Class showing multiple inheritance
abstract class CC implements A, B {

	public function __construct() {

		echo "In CC constructor.";
	}

	// Public Method - Add
	public function add( int $a, int $b ) : int {

		return $a + $b;
	}

	// Public Method - Sub
	public function sub( int $a, int $b ) : int {

		return $a - $b;
	}
}

// An Abstract Class showing multiple inheritance
abstract class CD extends CC implements C, D {

	public function __construct() {

		echo "In CD constructor.";
	}

	// Public Method - Multiply
	public function multiply( int $a, int $b ) : int {

		return $a * $b;
	}
}

// Concrete Class
class CE extends CD {

	public function __construct() {

		echo "In CE constructor.";
	}

	// Public Method - Division
	public function division( int $a, int $b ) : float {

		$result = 0;

		try {

			$result = $a / $b;
		}
		catch(DivisionByZeroError $ex) {

			echo $e->getMessage();
		}

		return $result;
	}
}
```

## Declaration

The important points to be taken care of while declaring an Interface or Abstract Class are listed below.

The Interface can be declared using the modifier interface.

The Abstract Class can be declared using the modifiers abstract and class. It's mandatory to use the abstract modifier for all the abstract classes.

Both the Interface and Abstract Class cannot use the access modifiers i.e. public, protected, and private.

Both the Interface and Abstract Class cannot use the final modifier which contradicts their abstraction.

## Variables

The Interface can only declare and define the constants using the const modifier.

The Interface cannot declare the variables using the access modifiers i.e. public, protected, and private.

The variables declared in an Abstract Class can be static, public, protected, or private as shown in the Class CA.

Both the Interface and Abstract Class cannot use the final modifier to declare the variables.

## Functions

The functions declared in an Interface are inherently public and abstract as shown in Interface B. It's optional to use the public and abstract modifiers.

The Interface functions cannot use the private and protected access modifiers.

The functions declared in an Abstract Class can be final, static, public, protected, or private. The Abstract Classes can always have abstract and non-abstract functions.

The abstract functions of an Abstract Class must always use the abstract keyword.

The function signature of classes implementing the interface must be compatible with LSP (Liskov Substitution Principle). The function declaration of the class and interface must be the same for the same function.

## Scope

We cannot declare private or protected Interfaces and Abstract Classes.

The Interfaces and Abstract Classes can be made visible to other namespaces or PHP scripts by importing them using the use keyword.

The Interfaces and Abstract Classes can be used directly within the same namespace without importing them.

## Inheritance

An Interface can extend either single or multiple Interfaces.

An Abstract Class can extend only a single Abstract or Concrete Class.

## Implementation

An Interface can be implemented by an Abstract Class using the keyword implements.

An Abstract Class can implement multiple Interfaces.

An Abstract Class can extend the Abstract or Concrete Class using the keyword extends.

An Interface cannot implement another Interface or Abstract Class.

## Usage

We can use an Abstract Class by partially implementing the class leaving the rest of the implementation to the child classes.

We can use an Interface to declare the methods only, leaving the implementation to the classes in their way.

We can use an Interface to declare the Global Variables.

We can use Interfaces to achieve multiple inheritance in classes.

## Summary

![Interface VS Abstract]({{site.baseurl}}/assets/img/php-interface-vs-abstract-class.png)