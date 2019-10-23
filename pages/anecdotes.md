---
layout: page
title: Anecdotes
description: 杂谈轶事
keywords: 轶事
comments: false
menu: 轶事
permalink: /anecdotes/
---

[人工智能之父图灵]({{ site.url }}/anecdotes/人工智能之父图灵/)

>作为IT男一枚，首先要感谢计算机科学之父-图灵。


---
<ul class="listing">
{% for anecdote in site.anecdotes %}
{% if anecdote.title != "TEMPLATE" and anecdote.title != "人工智能之父图灵" %}
{% if anecdote.original == true %}
<li class="listing-item"><a href="{{ site.url }}{{ anecdote.url }}">(原创){{ anecdote.title }}</a></li>
{% else %}
<li class="listing-item"><a href="{{ site.url }}{{ anecdote.url }}">(收录){{ anecdote.title }}</a></li>
{% endif %}
{% endif %}
{% endfor %}
</ul>







