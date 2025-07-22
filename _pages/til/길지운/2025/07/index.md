---
layout: post
title: "TIL - 길지운"
permalink: /til/길지운/
---

<ul>
  {% assign posts = site.posts | where_exp: "post", "post.path contains '_posts/til/길지운'" %}
  {% for post in posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
