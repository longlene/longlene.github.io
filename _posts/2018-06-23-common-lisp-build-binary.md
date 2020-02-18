---
layout: post
title: Common Lisp Build Binary
date: 2018-06-23 14:24:02 +0800
tags: [lisp]
---
也许在大家的印象中，Common Lisp一直是个解释型语言；但是其实各个实现基本都实现了导出可执行文件的功能。

但是，各个Common Lisp实现都有各自不同的导出函数；这种糟糕的境况在最近的asdf更新后得到了解决。

下面通过一个minimal 程序演示asdf生成可执行文件的过程。

1. 安装Common Lisp实现
SBCL或Clozue CL，或其他你喜欢的实现。
`emerge sbcl or clozurecl`, or use other package manager

2. asdf3会在默认会在以下路径查找system
~/common-lisp/ or ~/.local/share/common-lisp/source/

3. 编译项目
进入~/.quicklisp/local-projects/目录，新建项目:hello
```bash
mkcd ~/.local/share/common-lisp/source/hello
vim hello.asd
```

为了最小化，代码直接写在asd文件中：hello.asd
```common-lisp
(defsystem hello
 :build-operation program-op
 :build-pathname "hello"
 :entry-point "hello:main")


(defpackage :hello
 (:use :cl)
 (:export #:main))

(in-package :hello)

(defun main ()
 (format t "hello, ~{~a ~}~%" (uiop:command-line-arguments)))
```

生成可执行文件
```bash
sbcl --oninform --eval '(asdf:make :hello)'
```

执行命令后会在hello目录生成可执行文件：hello

查看hello的链接情况：`ldd hello`
输出如下：
```bash
        linux-vdso.so.1 (0x00007ffc595f4000)
        libdl.so.2 => /lib64/libdl.so.2 (0x00007fa195895000)
        libpthread.so.0 => /lib64/libpthread.so.0 (0x00007fa195679000)
        libz.so.1 => /lib64/libz.so.1 (0x00007fa195462000)
        libm.so.6 => /lib64/libm.so.6 (0x00007fa19515f000)
        libc.so.6 => /lib64/libc.so.6 (0x00007fa194dc6000)
        /lib64/ld-linux-x86-64.so.2 (0x00007fa195a99000)
```


