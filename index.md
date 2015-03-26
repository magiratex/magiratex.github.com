---
layout: page
title: Welcome!
tagline: 從別後，憶相逢，幾回魂夢與君同。今宵剩把銀釭照，猶恐相逢是夢中。
---
{% include JB/setup %}

![Alt text](http://cmcs.fzu.edu.cn/images/logo.gif "Optional title")
    
## Latest

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>



