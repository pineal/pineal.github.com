---
title: Categories
layout: blog
---

<ul class="listing">

{% for cat in site.categories %}
{% if cat[0] != "gutter"%}
  <li class="listing-seperator" id="{{ cat[0] }}"><h4>__{{ cat[0] }}__[{{ cat[1].size }}]</h4></li>
{% for post in cat[1] %}
  <li class="listing-item">
  <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
  <a href="{{ site.url }}{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
  </li>
{% endfor %}
{% endif %}
{% endfor %}
</ul>
