---
layout: post
title: Common-Lisp-Build-Binary
---
在大家的印象中，Common Lisp一直是个解释型语言；但是其实各个实现基本都实现了导出可执行文件的功能。

但是，各个Common Lisp实现，都有各自不同的导出函数；这种糟糕的境况在最近的asdf更新后得到了解决。

下面通过一个最小化项目演示asdf生成可执行文件的过程。

1. 安装Common Lisp实现
SBCL或Clozue CL，其他实现暂未测试，应该也可以。

2. 安装quicklisp
quicklisp可以自动查找其local-projects目录中的项目，能够简化开发。
这里quicklisp安装路径是~/.quicklisp/

3. 编译项目
进入~/.quicklisp/local-projects/目录，新建项目:hello
```shell
mkcd ~/.quicklisp/local-projects/hello
vim hello.asd
```

为了最小化，代码直接写在asd文件中：hello.asd
```lisp
(defsystem hello
 :build-operation program-op
 :build-pathname "hello"
 :entry-point "hello:main")


(defpackage :hello
 (:use :cl)
 (:export #:main))

(in-package :hello)

(defun main (&rest args)
 (declare (ignorable args))
 (format t "hello, world~%"))
```

生成可执行文件
```shell
sbcl --eval '(asdf:make :hello)'
```

执行命令后会在hello目录生成可执行文件：hello

