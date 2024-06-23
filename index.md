---
layout: default
title: Kengo's Blog
description: Thoughts on software, machine learning, geography, chess, basketball, and more.
---

{% assign totalPosts = site.posts | size %}
{% assign totalWords = 0 %}

{% for post in site.posts %}
  {% assign postWords = post.content | number_of_words %}
  {% assign totalWords = totalWords | plus: postWords %}
{% endfor %}

<h2>Hi, I'm Kengo</h2>

... and this is my blog, Kengo's Blog.

I like building scalable systems by day and trying to reinvent the wheel by night. I had long been looking for a quiet place on the Internet to document my experiments and thoughts on various topics, and finally decided to start a blog in June 2024.

I live in London and work as a software engineer at Bloomberg. Previously I worked for a MLOps startup, also in London, and prior to that I was at a marketplace company in Tokyo, where I started my career in 2020.

The minimal design of this website is _heavily_ inspired by [Alex Molas's blog], whose content I also highly recommend.


[Alex Molas's blog]: https://www.alexmolas.com/

---

<h2>Select Posts</h2>

{% for post in site.posts %}
{% if post.feature %}
  <li>
    <span class="post-date">{{ post.date | date: "%b %d %Y" }}</span> · <a href="{{ post.url }}">{{ post.title }}</a>
  </li>
{% endif %}
{% endfor %}
