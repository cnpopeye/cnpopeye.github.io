---
layout: default
title: CATEGORIES
---

<div id='tag_cloud'>
{% for cat in site.categories %}
<a href="#{{ cat[0] }}" title="{{ cat[0] }}" rel="{{ cat[1].size }}">{{ cat[0] }} ({{ cat[1].size }})</a>
{% endfor %}
</div>

<ul class="posts">
{% for cat in site.categories %}
   <li class="listing-seperator" id="{{ cat[0] }}">{{ cat[0] }}</li>
   {% for post in cat[1] %}
      <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
  </li>
  {% endfor %}
{% endfor %}
</ul>
