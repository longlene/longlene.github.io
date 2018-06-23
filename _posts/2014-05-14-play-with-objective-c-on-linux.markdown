---
layout: post
title: "Play with Objective-C on Linux"
date: 2014-07-27 01:40:00 +0800
comments: true
categories: Programming
---

Sun May 4 23:46:54 2014  

Ojbective-C is the main programmming language on OS X.
What should we do if we wanna play with it with the Foundation framework on Linux?  
We can install the GNUStep platform which is inspired by NextStep in the past. We can install gcc with objc support and gnustep-base package. Here I've tried another solution which use the clang and cocotron library.

First, install clang compiler and libobjc2 runtime library to compile the Objective-C source file.  
Second, follow the cocotron readme file, install blocksruntime library.  
Third, install the cocotron library.

If you use Gentoo/Funtoo, you are the lucky dog as I've wrote some ebuilds for installing the cocotron lirary. [https://github.com/longlene/clx](https://github.com/longlene/clx/tree/master/dev-libs/cocotron).  
You can add it to your system and just use the following command to install blocksruntime and cocotron package:
`emerge cocotron`.

After installing the library, you can use all the class from the Foundation library as same as the Foundation framework on OS X.

Now, we can try our first program--printing the famous "hello, world"

hello.m  
```objc
#include <Foundation/Foundation.h>

int main(int argc, char *argv[])  
{  
    @autoreleasepool {
            NSLog(@"hello, world");
    }
    return 0;
}
```


On OS X, We can compile the file by the following command:  
`clang -o hello hello.m -framework Foundation`

On Linux, commands like this:  
`clang -o hello hello.m -lFoundation`

Run it and we'll see the output:
`
2014-05-04 12:59:22.314 [5282:7f6f3734d700] hello, world
`

Yeah! We've done it.

With cocotron, we can use almost all the classes with "NS" suffix in the Foundation framework. The cocotron framework may be not stable on Linux, so as far as now, we could only write command-line toos with cocotron on Linux.

The cocotron's project homepage: [https://code.google.com/p/cocotron](https://code.google.com/p/cocotron)

20140514_234800
