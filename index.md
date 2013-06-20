---
layout: page
title: LANNIER'S　BLOG
tagline: Make it works first
---

{% assign listing_limit = 5 %}
{%for post in site.posts limit: listing_limit%}
<div class="post_summary">
  <h2><a href="{{post.url}}">{{post.title}}</a></h2>
  <div>
    {{ post.content }}
  </div>
<!--   <div class="post_footer">
    <p><a href="{{post.url}}">Read more ...</a></p>
  </div> -->
</div>
<span><strong>{{ post.date | date_to_string }}</strong></span>
<hr/>
{%endfor%}


<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
