---
layout: post
title: "Introduction of C++ Interesting Facts and Preprocessors (6)"
date: 2021-02-28 00:06:00 +0800
description: "Introduction of C++ Interesting Facts and Preprocessors (6)" # (optional)
img: 2021-02-28-cover-image-of-cpp-notes.jpg # Add image post (optional)
fig-caption: "Introduction of C++ Interesting Facts and Preprocessors (6)" # Add figcaption (optional)
tags: ['Programming', 'C++']
categories: ['Programming', 'C++']
---

> In a C program, all lines that start with # are processed by preprocessor which is a special program invoked by the compiler. by this we mean to say that the ‘#’ symbol is used to process the functionality prior than other statements in the program, that is, which means it processes some code before run-time or say during the compile-time. In a very basic term, preprocessor takes a C program and produces another C program without any #.

The following are some interesting facts about preprocessors in C. 

1) When we use include directive,  the contents of included header file (after preprocessing) are copied to the current file. 
Angular brackets < and > instruct the preprocessor to look in the standard folder where all header files are held.  Double quotes “ and “ instruct the preprocessor to look into the current folder (current directory). 

2) When we use define for a constant, the preprocessor produces a C program where the defined constant is searched and matching tokens are replaced with the given expression. For example in the following program max is defined as 100.

```cpp
#include<stdio.h>
#define max 100
int main()
{
    printf("max is %d", max);
    return 0;
}

// OUTPUT
// max is 100
```

3) The macros can take function like arguments, the arguments are not checked for data type. For example, the following macro INCREMENT(x) can be used for x of any data type.

```cpp
#include <stdio.h>
#define INCREMENT(x) ++x
int main()
{
    char *ptr = "GeeksQuiz";
    int x = 10;
    printf("%s  ", INCREMENT(ptr));
    printf("%d", INCREMENT(x));
    return 0;
}

// output
// eeksQuiz  11
```

4) The macro arguments are not evaluated before macro expansion. For example, consider the following program

```cpp
#include <stdio.h>
#define MULTIPLY(a, b) a*b
int main()
{
    // The macro is expanded as 2 + 3 * 3 + 5, not as 5*8
    printf("%d", MULTIPLY(2+3, 3+5));
    return 0;
}
// Output: 16
```

The previous problem can be solved using following program 

```cpp
#include <stdio.h>
//here, instead of writing a*a we write (a)*(b)
#define MULTIPLY(a, b) (a)*(b)
int main()
{
    // The macro is expanded as (2 + 3) * (3 + 5), as 5*8
    printf("%d", MULTIPLY(2+3, 3+5));
    return 0;
}
// This code is contributed by Santanu
// Output: 40
```

5) The tokens passed to macros can be concatenated using operator ## called Token-Pasting operator.

```cpp
#include <stdio.h>
#define merge(a, b) a##b
int main()
{
    printf("%d ", merge(12, 34));
}

// output: 1234
```

6) A token passed to macro can be converted to a string literal by using # before it.

```cpp
#include <stdio.h>
#define get(a) #a
int main()
{
    // GeeksQuiz is changed to "GeeksQuiz"
    printf("%s", get(GeeksQuiz));
}

// output GeeksQuiz
```

7) The macros can be written in multiple lines using ‘\’. The last line doesn’t need to have ‘\’.

```cpp
#include <stdio.h>
#define PRINT(i, limit) while (i < limit) \
                        { \
                            printf("GeeksQuiz "); \
                            i++; \
                        }
int main()
{
    int i = 0;
    PRINT(i, 3);
    return 0;
}

// Output: 
// GeeksQuiz GeeksQuiz GeeksQuiz
```

8) The macros with arguments should be avoided as they cause problems sometimes. And Inline functions should be preferred as there is type checking parameter evaluation in inline functions. From C99 onward, inline functions are supported by C language also. 

For example consider the following program. From first look the output seems to be 1, but it produces 36 as output.

```cpp
#include <stdio.h>
 
#define square(x) x*x
int main()
{
    // Expanded as 36/6*6
    int x = 36/square(6); 
    printf("%d", x);
    return 0;
}

// output 36
```

If we use inline functions, we get the expected output. Also, the program given in point 4 above can be corrected using inline functions.

```cpp
#include <stdio.h>
 
static inline int square(int x) { return x*x; }
int main()
{
	int x = 36/square(6);
	printf("%d", x);
	return 0;
}

// output 1
```

9) Preprocessors also support if-else directives which are typically used for conditional compilation. 

```cpp
int main()
{
#if VERBOSE >= 2
	printf("Trace Message");
#endif
}

// output: No Output
```

10) A header file may be included more than one time directly or indirectly, this leads to problems of redeclaration of same variables/functions. To avoid this problem, directives like defined, ifdef and ifndef are used. 

11) There are some standard macros which can be used to print program file (__FILE__), Date of compilation (__DATE__), Time of compilation (__TIME__) and Line Number in C code (__LINE__)

```cpp
#include <stdio.h>
 
int main()
{
   printf("Current File :%s\n", __FILE__ );
   printf("Current Date :%s\n", __DATE__ );
   printf("Current Time :%s\n", __TIME__ );
   printf("Line Number :%d\n", __LINE__ );
   return 0;
}

// output:
// Current File :/usr/share/IDE_PROGRAMS/C/other/081c548d50135ed88cfa0296159b05ca/081c548d50135ed88cfa0296159b05ca.c
// Current Date :Sep  4 2019
// Current Time :10:17:43
// Line Number :8
```

12) We can remove already defined macros using : ```#undef MACRO_NAME ```

```cpp
#include <stdio.h>
#define LIMIT 100
int main()
{
	printf("%d",LIMIT);
	//removing defined macro LIMIT
	#undef LIMIT
	//Next line causes error as LIMIT is not defined
	printf("%d",LIMIT);
	return 0;
}
//This code is contributed by Santanu
```

Following program is executed correctly as we have declared LIMIT as an integer variable after removing previously defined macro LIMIT 

```cpp
#include <stdio.h>
#define LIMIT 1000
int main()
{
	printf("%d",LIMIT);
	//removing defined macro LIMIT
	#undef LIMIT
	//Declare LIMIT as integer again
	int LIMIT=1001;
	printf("\n%d",LIMIT);
	return 0;
}

// output:
// 1000
// 1001
```

Another interesting fact about macro using (#undef)  

```cpp
#include <stdio.h>
//div function prototype
float div(float, float);
#define div(x, y) x/y
 
int main()
{
//use of macro div
//Note: %0.2f for taking two decimal value after point
printf("%0.2f",div(10.0,5.0));
//removing defined macro div
#undef div
//function div is called as macro definition is removed
printf("\n%0.2f",div(10.0,5.0));
return 0;
}
 
//div function definition
float div(float x, float y){
return y/x;
}
//This code is contributed by Santanu

// output
// 2.00
// 0.50
```