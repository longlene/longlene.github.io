---
layout: post
title: Clojure with GraalVM
date: 2018-06-23 19:44:02 +0800
tags: [clojure, programming]
---
## 缘起
最近接触到了GraalVM这个项目，发现非常强大，不仅能够编译java代码，输出可执行文件，而且应该能够输出成为库文件，供C系语言使用。如果能成功的话，以后Java系软件的发布应该能大大简化。

这些天试玩了下Clojure，想试试用GraalVM编译下如何。

搜了下，果然，已经有人做了lein的插件 [lein-native-image](https://github.com/taylorwood/lein-native-image)

下面是使用lein插件编译Clojure代码的过程

## 新建项目
使用lein新建项目：hello
`lein new hello`

编辑proejct.clj，导入native-image插件
```clojure
(defproject hello "0.1.0-SNAPSHOT"
  :description "hello graal demo"
  :dependencies [[org.clojure/clojure "1.8.0"]]
  :jvm-opts ["-Dclojure.compiler.direct-linking=true"]
  :plugins [[io.taylorwood/lein-native-image "0.2.0"]]
  :target-path "target/%s"
  :native-image {:graal-bin "/opt/graal-bin-1.0.0_rc2/bin"
  ;               :opts ["--verbose"]
                 :name "main"}
  :main hello.core
  :profiles {:uberjar {:aot :all}})
```

编辑代码src/hello/core.clj
```clojure
(ns hello.core
  (:gen-class))

(defn -main [& args]
  (println "hello, world"))
```

## 运行项目
使用lein运行项目
`lein run`
输出："hello, world"

## 编译项目
运行如下命令编译项目
`lein native-image`

## 结果
运行编译命令将会调用Graal的native-image命令生成可执行文件
本地中会在target/default目录下生成可执行文件**hello**


