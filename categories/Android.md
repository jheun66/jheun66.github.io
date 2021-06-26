---
layout: category
title: Android
description: "안드로이드 개발 정리"
background: '/img/bg-android.jpg'
permalink: '/categories/Android'
pagination: 
  enabled: true
  category: Android
---

<!-- Category Post List -->
{% for post in paginator.posts %}

<article class="posts-list">
    <a href="{{ site.baseurl }}{{ post.url }}">
    <h2 class="post-title">{{ post.title }}</h2>
    {% if post.subtitle %}
    <h3 class="post-subtitle">{{ post.subtitle }}</h3>
    {% endif %}
    </a>
    <p class="post-meta">Posted by
    {% if post.author %}
    {{ post.author }}
    {% else %}
    {{ site.author }}
    {% endif %}
    on
    {{ post.date | date: '%B %d, %Y' }}           
    </p>
</article>

<hr>

{% endfor %}

<!-- Pager -->
{% if paginator.total_pages > 1 %}

<div class="clearfix">

    {% if paginator.previous_page %}
    <a class="btn btn-primary float-left" href="{{ paginator.previous_page_path | prepend: site.baseurl | replace: '//', '/' }}">&larr;
    Newer<span class="d-none d-md-inline"> Posts</span></a>
    {% endif %}

    {% if paginator.next_page %}
    <a class="btn btn-primary float-right" href="{{ paginator.next_page_path | prepend: site.baseurl | replace: '//', '/' }}">Older<span class="d-none d-md-inline"> Posts</span> &rarr;</a>
    {% endif %}

</div>

{% endif %}