---
layout: page
title: Links
description: 快速定位
keywords: 链接
comments: false
menu: 链接
permalink: /links/
---

> 网络资源

{% for link in site.data.links %}
  {% if link.src == 'www' %}
* [{{ link.name }}]({{ link.url }})
  {% endif %}
{% endfor %}

> 常用软件

{% for link in site.data.links %}
  {% if link.src == 'software' %}
* [{{ link.name }}]({{ link.url }})
  {% endif %}
{% endfor %}

> 自制小工具

{% for link in site.data.links %}
  {% if link.src == 'tool' %}
* [{{ link.name }}]({{ link.url }})
  {% endif %}
{% endfor %}
