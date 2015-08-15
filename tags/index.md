---
title: Tags
layout: blog
---

<div id='tag_cloud'>
{% for tag in site.tags %}
<li><a href="#{{ tag[0] }}" title="{{ tag[0] }}" rel="{{ tag[1].size }}">{{ tag[0] }}</a></li>
{% endfor %}
</div>

<ul class="listing" id＝"tagList">
{% for tag in site.tags %}
  <li class="listing-seperator" id="{{ tag[0] }}"><h4>__{{ tag[0] }}__[{{tag[1].size}}]</h4></li>
{% for post in tag[1] %}
  <li class="listing-item">
  <a href="{{ site.url }}{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
  </li>
{% endfor %}
{% endfor %}
</ul>

<script src="/media/js/jquery.tagcloud.js" type="text/javascript" charset="utf-8"></script>
<script language="javascript">
$.fn.tagcloud.defaults = {
    size: {start: 1, end: 1, unit: 'em'},
      color: {start: '#000000', end: '#cccccc'}
};

$(function () {
    $('#tag_cloud a').tagcloud();
});
</script>
