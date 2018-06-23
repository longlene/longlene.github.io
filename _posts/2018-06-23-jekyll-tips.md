---
layout: post
title: jekyll tips
---
自从从octopress改用jekyll之后，好久没写blog了。这次重新拾起来，把插件安装好，顺便把使用方法记录下来。

这次安装了jekyll-compose插件来创建草稿和发布文章。

## 安装jekyll-compose
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
