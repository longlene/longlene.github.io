---
layout: post
title: Jekyll Tips
date: 2018-06-23 12:03:02 +0800
tags: [jekyll]
---
自从从octopress改用jekyll之后，好久没写blog了。这次重新拾起来，把插件安装好，顺便把使用方法记录下来。

## 安装jekyll到本地
```bash
gem install --user-install jekyll bundler
```

## 新建博客
```bash
jekyll new longlene.github.io
```

## Jekyll Compose
jekyll-compose会额外提供一些命令，用于创建草稿。
在Gemfile中增加配置
```ruby
group :jekyll_plugins do
  gem "jekyll-compose", "~> 0.8.0"
end
```
然后运行`bundle install`

## 新建草稿
```sh
bundle exec jekyll draft "jekyll tips"
```

## 发布文章
```sh
bundle exec jekyll publish _drafts/jekyll-tips.md
```

## 本地预览
```sh
bundle exec jekyll serve
```
