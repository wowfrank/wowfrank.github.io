---
layout: post
title: "Go Notes of Package and Module (1)"
date: 2020-11-26 00:01:00 +0800
description: "Go Notes of ackage and Module" # (optional)
img: cover-image-of-golang-notes-series.png # Add image post (optional)
fig-caption: "Go Notes of ackage and Module" # Add figcaption (optional)
tags: ['Programming', 'Golang']
categories: ['Programming', 'Golang']
---

## Overview

Package is a way of code reusability in GO. As the name suggests, it is a way of grouping related code. Go modules is a way of dealing with dependencies in golang. 

## PACKAGES

All .go files present in the same directory will belong to the same package. A great misconception around packages is that package is the name of the directory which contains .go files. That is not correct. A directory is just a directory and the name of the package is what is present in Package Declaration. Then what is the importance of the directory name? It will be explained in the tutorial as we go along.

Every GO source file (.go file) in a GO application file belongs to a package. That is why every .go file starts with.

### Package can be of two types.

- Executable package – Only main is the executable package in GoLang. A .go file might belong to the main package present within a specific directory. We will see later how the directory name or the .go file name matters.  The main package will contain a main function that denotes the start of a program. On installing the main package it will create an executable in the $GOBIN directory.

- Utility package– Any package other than the main package is a utility package. It is not self-executable. It just contains the utility function and other utility things that can be utilized by an executable package.
All .go files present in the same directory will belong to the same package. This is true for all directory containing packages. It doesn’t matter whether that directory contains the go.mod file or not.   Let’s validate that. Change the package declaration to

## Modules

Module is go support for dependency management. A module by definition is a collection of related packages with go.mod at its root.  The go.mod file defines the

- Module import path.

- Dependency requirements of the module for a successful build. It defines both project’s dependencies requirement and also locks them to their correct version

## Applications

All .go files present in the same directory will belong to the same package. This is true for all directory containing packages. It doesn’t matter whether that directory contains the go.mod file or not.   Let’s validate that. Change the package declaration to

GO source files belonging to different packages with in the same directory is not allowed hence this error.


**Importing of a package**

- main.go file imports the package using “sample.com/learn/math” and is able to call Add and Subtract using math.Add(..) and math.Subtract(..)See how we have imported the math package in the main.go file.

- Use of the directory is for import statements in GO program. In import, we provide the directory path and not the package name. GO then fetches all the files having the same package name.

## Nested Packages

In GO it is possible to create nested packages. Let’s create a new directory named advanced inside math directory.  “. This directory will contain square.go file which will package declaration as “package advanced”

**learn/main.go**

```go
package main
import (
    "fmt"
    "sample.com/learn/math"
    "sample.com/learn/math/advanced"
)
func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math.Subtract(2, 1))
    fmt.Println(advanced.Square(2))
}
```
Points to note about above program

- We imported the advanced package in main.go with full qualified path i.e,  import “sample.com/learn/math/advanced”

- Square function is referred using advanced package i.e, advanced.Square(2)

- As mentioned earlier directory name can be other advanced just that it has to be imported accordingly

- Also filename can be anything other than advanced.go

## Init Functions

init() function is a special function that is used to initialize global variables of a package. These functions are executed when the package is initialized. Each of the GO source files in a package can have its own init() function. Whenever you import any package in the program, then on the execution of that program, init functions(if present)  in the GO source files belonging to that imported package are called first. Some points to note about init function

- Init function is optional

- Init function does not take any argument

- Init function does not have any return value.

- Init function is called implicitly. Since it is called implicitly, init function cannot reference it from anywhere.

- There can be multiple init() functions within the same source file.

init function is majorly used for the initialization of global variables that cannot be initialized using an initialization expression. For example, it requires a network call to intialize any DB client. Another example could be fetching secret keys on startup. Init function is also used for running anything that only needs to be executed once. Let’s see a simple use case of using an init function. 

## Order of execution of a Go program

Below the order of execution of a go program.

- The program starts with the main package.

- All imported packages in the source files of the main package are initialized. The same thing happens recursively for further imported packages.

- Then global variables declaration in these packages is initialized. The initialization dependency kicks in for the initialization of these variables. https://golang.org/ref/spec#Order_of_evaluation

- After this, init() function is run in these packages

- Global variables in the main package are initialized

- init function in the main package is run if present

- main function in main package is run.

Note here that package initialization is only done once even if it is imported several times. 

## Blank Identifier in import

Blank identifier in importing packages means specifying a blank import for the imported package. The syntax for it is

```go
import _ 
```

What is this blank import and why it is used. For this, you have to understand two things

1. About init function

2. About blank identifier represented by an underscore (‘\_‘)

So now a blank import of a package is used when

1. The imported package is not being used in the current program

2. But we intend to import that package so that the init function in the GO source files belonging to that package can be called and initialization of variables in that package can be done properly

So basically a blank import is used when a package is solely imported for its side effects. As an example MySQL package is used as a blank import for its side-effect of registering the MySQL driver as a database driver in the init() function of MySQL package, without importing any other functions:

```go
_ "github.com/go-sql-driver/mysql"
```

## Package Naming Convention

A good name is very important for the package as any access to the package’s types, functions, constants, or variables is prefixed by the package name. So the package name should be short and clear. It is recommended to avoid

- Underscore in the package name

- Camel casing or any kind of mixed caps

## Types of Modules

We learn that module is a directory containing nested go packages. So essentially module can be treated as a package only that contains nested packages. We have seen in the package tutorial can a package can be either an executable package or utility package (non-executable). Similar to package, modules can be of two types.

- Executable module – We already know that main is the executable package in GoLang. Hence a module containing the main package is the executable module .  The main package will contain a main function that denotes the start of a program. On installing the module having main package it will be create an executable in the $GOBIN directory.

- Non-Executable module or Utility Module– Any package other than main package is a non-executable package. It is not self executable. It just contains the utility function and other utility things which can be utilized by an executable package. Hence if the module doesn’t contain the main package then it will be a non-executable or utility module.  This module is meant to be used as a utility and will be imported by other modules.

To create a executable for a module  (Only for module with main package)

- Do a go build and it will create the executable in the current directory

- Do a go install and it will create the executable in the $GOBIN directory

## Package vs Module

As per module definition, it is a directory containing a collection of nested and related go packages go.mod at its root.  The go.mod file defines the

- Module import path.

- Dependency requirements of the module for a successful build. It defines both project’s dependencies requirement and also locks them to their correct version

Modules provides

- Dependency Management

- With modules go project doesn’t necessarily have to lie the $GOPATH/src folder. 

## Add a dependency to your project

Let’s explore some ways of adding dependency to your project

- Directly adding it to the go.mod file

- Do a go get: go get -u github.com/go-sql-driver/mysql

- Add the dependency to your source code and do a go mod tidy

Before looking at each of the ways, again let’s create a module first

```go
go mod init sample.com/learn
```

### Directly adding it to the go.mod file

Add below dependency to the go.mod file

```go
module sample.com/learn

go 1.14

require github.com/pborman/uuid v1.2.1
```

```go
go mod download
```

### Do a go get

```go
export GO111MODULE=on
go get github.com/pborman/uuid
```

The dependency will be marked as //indirect as it is not being used in any of the source files. Once you do a go build after using this in the source files, the //indirect will be removed automatically by go. Also it will update the go.sum file with the checksum and version of all direct and indirect dependencies.

### Add the dependency to your source code and do a go mod tidy

Basically go mod tidy command makes sure that your go.mod files reflects the dependencies that you have actually used in your project. When we run go mod tidy command then it will do two things

- Add any dependency which is imported in the source files

- Remove any dependency which is mentioned in the go.mod file but not imported in any of the source files.

```go
go mod tidy
```

## Adding a vendor directory

If you want to vendor your dependencies,  then below command can be used to achieve the same

```go
go mod vendor
```

It will create a vendor directory inside your project directory. You can also check in the vendor directory to your VCS (Version Control System). This becomes useful in sense that none of the dependency needs to be downloaded at run time as it is already present in the vendor folder checked into VCS

## Module Import Path

### There can be three cases that decide what import path name can be used with modules.

- The module is a utility module and you plan to publish your module

- The module is a utility module and you don’t plan to publish your module

- The module is a executable module

### The module is a utility module and you plan to publish your module

If you plan to publish your module then the module name should match the URL of the repo which host that module. Go tries to download dependencies from the VCS using the same import path of the module.

### The module is a utility module and you don’t plan to publish your module

This is the case when you only mean to use the utility module locally only. In this case the import path can be anything.

### The module is a executable module

In this case also module import path can be anything. The module import path can be a non-url even if you plan to commit your module into VCS as it will not be used by any other module

However it is a good practice to use meaningful import path while creating module

## Importing package within same module

Any package within the same module can be imported using the import path of module + directory containing that package. To illustrate lets create a module


- Make a learn directory

- Create a module with import path as “sample.com/learn”

```go
go mod init sample.com/learn
```

- Now create main.go (Having main package and main function)

- And math/math.go – math package

```go
// main.go file
package main

import (
	"fmt"
	"sample.com/learn/math"
)

func main() {
	fmt.Println(math.Add(1, 2))
}
```

```go
// math/math.go
package math

func Add(a, b int) int {
    return a + b
}
```

See how we have imported the math package in the main.go file

```go
"sample.com/learn/math"
```

Here the import path is import path of module which is sample.com/learn +  directory containing the package which is math. Hence “sample.com/learn/math” . Packages in nested directory can also be imported in the same way. The way it works is that since the prefix is the module import path, hence go will know that you are trying to import from the same module. So it will directly refer it instead of downloading it.

## Importing package from different module locally

There are cases when we want to import a module which is present locally. Let’s understand how we can import such a module. But first, we have to create a module that can be used by others and then import it into the other module. For that let’s create two modules

- sample.com/math module

- school module

school module will be calling code of the sample.com/math module

Let’s first create the sample.com/math module which will be used by school module

- Make a math directory

- Create a module with import path as sample.com/math

```go
go mod init sample.com/math
```

- Create a file math.go with below contents  in the math directory

```go
package math

func Add(a, b int) int {
	return a + b
}
```

Now let’s create the school module

- Now create a school directory in the same path as math directory side by side

- Create a module name school

```go
go mod init school
```

- Now let’s modify the go.mod file to import the math module in the school module. To import a local module that is not pushed to VCS, we are going to use replace directory. The replace directory will replace the module path with the path you specify.

```go
module school

go 1.14

replace sample.com/math => ../math
```

- Create file school.go which is going to use the Add function in sample.com/math module

```go
package main

import (
	"fmt"
	"sample.com/math"
)

func main() {
	fmt.Println(math.Add(2, 4))
}
```

Now do a go run

```go
go run school.go
```

It is able to call the Add function of the sample.com/math  module and correctly gives the output as 6.

Also it will update the go.mod with version information of the sample.com/math module

```go
module school

go 1.14

replace sample.com/math => ../math

require sample.com/math v0.0.0-00010101000000-000000000000
```

## Selecting the version of library

To understand how does GO’s approach while selecting the version of the library of which two versions are specified in the go.mod file, we have to first understand Semantic Versioning

Semantic Versioning is comprised of three parts separated by dots. Below is the format for versioning.

```go
v{major_version}.{minor_version}.{patch_version}
```

where

- **v** – it is just an indicator that it is a version

- major_version – It represents the incompatible API changes in the library. So when there are changes in the library that is not backward compatible, in that case, major_version is incremented

- minor_version – It represents the change in functionality of the library in a backward-compatible manner. So when there are some functionality changes in the library but those changes are backward compatible then, in that case, the minor version is incremented

- patch_version – It represents the bug fixes in the library in a backward-compatible manner. So when there are bug fixes to the existing functionality of the library, then in that case patch_version is incremented.
Now there can be two cases

- Two versions of the same library is used which only differ in the minor and patch version. Their major version is the same.

- Two versions of the same library is used which differ in the major.

Let’s see what approach does go follows in the above two cases

### Differ in minor or patch version

Go follows the minimum version policy approach while selecting the version of the library of which two versions are specified in the go.mod file which differ only in their minor or patch version.

For example in case you are using the two versions of same library which are `1.2.0` and `1.3.0`
then go will choose 1.3.0 as it is the latest version.

### Differ in major version

Go treats the major version as a different module itself. Now, what does that means? This essentially means that the import path will have a major version as its suffix. Let’s take the example of any go library with VCS as github.com/sample. Let’s latest semantic version is `v8.2.3`

Then the go.mod file will like below

```go
module github.com/sample/v8

go 1.13

..
```

It has major version in its import path. So any library which is using this sample library have to import it like

```go
import "github.com/sample/v8"
```

If in future v9 version is released than it has to be imported in the application like

```go
import "github.com/sample/v9"
```

Also the library will change its go.mod file to reflect the v9 major version

```go
module github.com/sample/v9
```

What it essentially allows is to use different major version of the same library to be used within same go application.  We can also give meaningful names when different major version of the same library is imported in the same application. For eg

```go
import sample_v8 "github.com/sample/v8"
import sample_v9 "github.com/sample/v9"
```

This is also known as Semantic Import Versioning

Also note that

- For the first version it is ok to not specify the version in the go.mod file.

- Also be careful when importing different major version of the same library. Look out for the new functionality that might be available with new versions.

Also for the same reason when you update a specific module using

```go
go get -u
```

then it will only upgrade to the latest minor version or patch version whichever applicable. For example let’s say the current version used by an application is

```go
v1.1.3
```

Also let’s say we have below versions available

```go
v1.2.0
v2.1.0
```

Then when we run

```go
go get
```

then it will update to

```go
v1.2.0
```

The reason is because go get will only update the minor or patch version but never the major version as go treats major version of a module as a different module entirely.

To upgrade the major version, specify that  upgraded dependency explicitly  in the go.mod file or do a go get of that version.

Also couple of points to note about upgrading module

- To upgrade a dependency to its latest patch version only, use below command
```go
go get -u=patch 
```

- To upgrade a dependency to a specific version, use below command
```go
go get dependency@version
```

- To upgrade a dependency to a specific commit, use below command
```go
go get @commit_number
```

- To upgrade all dependency to their latest minor and patch version, use below command
```go
go get ./...```

## go mod command

Below are some of the options for the go mod command.

- download – It will download the the required dependencies to the  $GOPATH/pkg/mod/cache folder.  Also it will update the go.sum file with the checksum and version of all direct and indirect dependencies

- edit – This is for editing the go.mod file. It provides a set of editing flags. Run below command to see set of all editing flags available

```go
go help mod edit
```

go help mod editFor eg below are some editing flags available

1. \-fmt flag will format the go.mod file. It will not make any other change

2. \-module flag can be used to set the module’s import path

- graph \– This can be used to print the module requirement dependency graph

- init \– We already have seen the usage of this command above. It is used to init a new module

- tidy \– This command will download all the dependencies that are required in your source files

- vendor \– If you want to vendor your dependencies,  then below command can be used to achieve the same. It will create a vendor directory inside your project directory. You can also check in the vendor directory to your VCS (Version Control System)

- verify \– This command checks for the modification of current downloaded dependencies. If any of the downloaded dependency has been verified that it will exit with a non-zero code

- why \–  this command analyzes the graph of packages from the main module. It prints the shortest path from the main module to the given package. For instance the school module which we created in section “Importing package from different module locally” if we print why command as below

```go
go mod why sample.com/math
```

then below will be the output

```go
# sample.com/math
school
sample.com/math
```

The output illustrates that the sample.com/math package is at one distance in the graph from main module which is school here.

## Direct vs Indirect Dependencies in go.mod file

A direct dependency is the dependency which the module directly imports . An indirect dependency is the dependency which are imported by module’s direct dependencies. Also, any dependency that is mentioned in the go.mod file but not imported in any of the source files of the module is also treated as an indirect dependency.

go.mod file only records the direct dependency.However it may record an indirect dependency in below cases

- Any indirect dependency which is not listed in the go.mod file of your direct dependency or if direct dependency doesn’t have a go.mod file , then that direct dependency will be added to the go.mod file with //direct as the suffix

- Any dependency which is not imported in any of the source file of the module (Example of this we have already seen earlier in the tutorial)

go.sum will record the checksum of direct and indirect dependencies.

Example of Indirect Dependencies in go.mod file

Let’s understand it with an example. For that let’s first create a module

```bash
git mod init sample.com/learn
```

Let’s add colly lib version v1.2.0 as a dependency in the go.mod file. colly version v1.2.0 doesn’t have a go.mod file.

```go
module sample.com/learn

go 1.14

require github.com/gocolly/colly v1.2.0
```

Now create a file learn.go

```go
// learn.go
package main

import (
	"github.com/gocolly/colly"
)

func main() {
	_ = colly.NewCollector()
}
```
Now do a go build. Since colly version v1.2.0 doesn’t have a go.mod file , all dependencies required by colly will be added to the go.mod file with //indirect as suffix. Do a go build. Now check the go.mod file. You will see below contents of the file

```go
module learn

go 1.14

require (
	github.com/PuerkitoBio/goquery v1.6.0 // indirect
	github.com/antchfx/htmlquery v1.2.3 // indirect
	github.com/antchfx/xmlquery v1.3.3 // indirect
	github.com/gobwas/glob v0.2.3 // indirect
	github.com/gocolly/colly v1.2.0
	github.com/kennygrant/sanitize v1.2.4 // indirect
	github.com/saintfish/chardet v0.0.0-20120816061221-3af4cd4741ca // indirect
	github.com/temoto/robotstxt v1.1.1 // indirect
	golang.org/x/net v0.0.0-20201027133719-8eef5233e2a1 // indirect
	google.golang.org/appengine v1.6.7 // indirect
)
```

All other dependencies are suffixed by //indirect. Also check that all direct and indirect dependencies will be recorded in the go.sum file.
