---
layout: post
title: "Jekyll Quick Start"
description: "翻译——jekyll快速入门"
category: jekyll
tags: [jekyll, quick start]
---
{% include JB/setup %}

参见 [Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)

更多的用法与完成的文档参见: [Jekyll Bootstrap](http://jekyllbootstrap.com)

## 准备

依次安装`ruby`、`gem`、`jekyll`,windows下的安装会有各种问题，google之。
关于jekyll的简介，官方文档（英文）[jekyll introduction](http://dyb.github.io/lessons/2011/12/29/jekyll-introduction/)

## 更新用户信息

记得在`_config.yml`更新自己的用户信息：
	
    title : My Blog =)
    
    author :
		name : Name Lastname
		email : blah@email.test
		github : username
		twitter : username

网站主题在需要的时候引用这些变量

## 简单的文章

主题默认包含简单的文章事例，当不需要时，删除 `_posts/core-samples` 文件夹

	$ rm -rf _posts/core-samples

这里是一个简单的文章列表

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


