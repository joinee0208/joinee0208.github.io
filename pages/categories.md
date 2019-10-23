---
layout: categories
title: Categories
description: 文章分类
keywords: 分类
comments: false
menu: 分类
permalink: /categories/
---

<section class="container posts-content">
{% assign sorted_categories = site.categories | sort %}
{% for category in sorted_categories %}
<h3>{{ category | first }}</h3>
<ol class="posts-list" id="{{ category[0] }}">
{% for post in category.last %}
<li class="posts-list-item">
<span class="posts-list-meta">{{ post.date | date:"%Y-%m-%d" }}</span>
 {% if post.original == true %}
<a class="posts-list-name" href="{{ site.url }}{{ post.url }}">(原创){{ post.title }}</a>
{% else %}
<a class="posts-list-name" href="{{ site.url }}{{ post.url }}">(收录){{ post.title }}</a>
{% endif %}
</li>
{% endfor %}
</ol>
{% endfor %}
</section>
<!-- /section.content -->
