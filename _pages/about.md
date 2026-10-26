---
layout: single
permalink: /
title: "互联网上的一粒沙子"
author_profile: false
redirect_from:
  - /about/
  - /about.html
---

{% include base_path %}

<p class="lede">这里按问题记工作笔记。一篇只回答一件以后还会再查的事。语言和工具写在标签里，笔记放进六架中的一架。</p>

<ol class="shelf-index">
{% for shelf in site.data.shelves %}
  <li>
    <a href="{{ base_path }}/notebook/#{{ shelf.slug }}">
      <span class="shelf-index__name">{{ shelf.title }}</span>
      <span class="shelf-index__blurb">{{ shelf.blurb }}</span>
    </a>
  </li>
{% endfor %}
</ol>

<h2 class="section-label">最近</h2>
{% if site.posts.size == 0 %}
<p>还没有笔记。</p>
{% else %}
{% for post in site.posts limit:5 %}
  {% include archive-single.html %}
{% endfor %}
<p class="shelf__more"><a href="{{ base_path }}/year-archive/">按时间看全部</a></p>
{% endif %}
