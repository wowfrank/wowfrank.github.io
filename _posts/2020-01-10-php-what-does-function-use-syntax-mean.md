---
layout: post
title: "PHP: What Does function ...() use () syntax mean?"
date: 2020-01-10 00:00:00 +0800
description: "PHP: What Does function ...() use () syntax mean?" # (optional)
img:  # Add image post (optional)
fig-caption: "PHP: What Does function ...() use () syntax mean?" # Add figcaption (optional)
tags: ['PHP','use']
categories: ['Programming', 'PHP']
---

I’ve recently seen an unfamiliar syntax all over the place, but I didn’t understand it and I couldn’t find anything about it while searching. Here’s a sample

```php
<?php
	// ...
	$loop->onReadable($server, function ($server) use ($loop) {
	    // ...
	});
?>
```

> The keyword ‘use’ has two different applications, but the reserved word table links to here. It can apply to namespace constucts.

Namespaces are pretty cool. They form the foundation of PSR-0, Composer, and everything good in the PHP these days. But, that syntax is used at the top of files on it’s own line. 

> The ‘use’ keyword also applies to closure constructs.

Closures: A (JavaScripty) World Within, Closures may also inherit variables from the parent scope. Any such variables must be passed to the use language construct. Inheriting variables from the parent scope is not the same as using global variables. Global variables exist in the global scope, which is the same no matter what function is executing. The parent scope of a closure is the function in which the closure was declared (not necessarily the function it was called from).

> Parameters by Any Other Name

Just like normal function parameters, parameters provided to the closure scope via the use keyword are passed by value. To pass parameters by reference, simply add an ampersand (&) in front of the parameter.

> Exciting Uses of Closures and the Use Keyword

```php
<?php
	// ...
	$httpServer = new ReactHttpServer($socketServer);
	$httpServer->on("request", function($request, $response) use ($repository, $docroot, $output) {
	    $path = $docroot.'/'.ltrim(rawurldecode($request->getPath()), '/');
	    if (is_dir($path)) {
	        $path .= '/index.html';
	    }
	    if (!file_exists($path)) {
	        HttpServer::logRequest($output, 404, $request);
	        $response->writeHead(404);
	        return $response->end();
	    }

	    // ...

	    $response->writeHead(200, array(
	        "Content-Type" => $type,
	    ));
	    $response->end(file_get_contents($path));
	});
	$socketServer->listen($port, '0.0.0.0');
	// ...
```

