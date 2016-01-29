---
layout: page
title: UnitTest++
tagline: is a lightweight unit testing framework for C++.
---
{% include JB/setup %}

It was designed to do test-driven development on a wide variety of platforms. Simplicity,
portability, speed, and small footprint are all very important aspects of UnitTest++. UnitTest++
is ANSI portable C++ and makes minimal use of advanced library and languages features, which
means it should be easily portable to just about any platform. Out of the box, the following
platforms are supported:

* Windows
* Linux
* Mac OS X

UnitTest++ was originally written by Noel Llopis and Charles Nicholson, and hosted on
SourceForge. It is now fully transitioned to GitHub, with the [official project here][repo].

[repo]: https://github.com/unittest-cpp/unittest-cpp "UnitTest++ Project Page"

### Latest Blog Post: <a href="{{ BASE_PATH }}{{ site.posts.first.url }}">{{ site.posts.first.title }}</a>
  
### Previous Blog Posts
<ul class="posts">
  {% for post in site.posts offset:1 %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
