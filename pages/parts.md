---
layout: page
title: Parts
description: 收集片段的想法
keywords: 片段, part
comments: false
menu: 片段
permalink: /parts/
---

> 记录点点滴滴！

<ul class="listing">
{% for part in site.parts %}
{% if part.title != "TEMPLATE" %}
{% if part.original == true %}
<li class="listing-item"><a href="{{ site.url }}{{ part.url }}">(原创){{ part.title }}</a></li>
{% else %}
<li class="listing-item"><a href="{{ site.url }}{{ part.url }}">(收录){{ part.title }}</a></li>
{% endif %}
{% endif %}
{% endfor %}
</ul>
