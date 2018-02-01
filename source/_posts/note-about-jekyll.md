---
title: 有关Jekyll的笔记
date: 2013-04-10 00:00:00 Z
categories:
  - article
tags:
  - jekyll
  - ruby
layout: post
description: ''
---

# Jekyll的安装

 .. . .... 自己google去~~

# 中文问题

无法生成页面，修改RUBY_HOME/bin/jekyll.bat，在顶部添加：

	SET LANG=zh_CN.UTF-8
	SET LC_ALL=zh_CN.UTF-8

新建的post中含有中文无法生成， 运行jekyll 报错：

> "Ruby193/lib/ruby/gems/1.9.1/gems/jekyll-0.11.2/lib/jekyll/convertible.rb:29:in ‘read_yaml’: invalid byte sequence in GBK (ArgumentError)"

找到ruby目录下gems/jekyll-0.11.0/lib/jekyll/convertible.rb29行修改为下面的内容:

	self.content = File.read(File.join(base, name), :encoding => "utf-8")


<!-- more -->

# 代码高亮

使用`highlight.js`插件，在页面端生成样式，或者也可以在文章中添加标记：

	{{ highlight html }}
	...
	{{ endhighlight }}

# Rakefile 命令

	rake preview

	rake page name=path/to/xxx.html

	rake post name=directory/title tags=tag1,tag2 date=2013-05-08
