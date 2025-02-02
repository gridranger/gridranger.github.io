---
layout: page
permalink: /posts
title: posts
---

{%- if site.posts.size > 0 -%}
    <div class="prompt">
      <span class="promptGreen">gridranger.github.io</span>:<span class="promptBlue">~</span> $ ls -l posts<br>
    </div>
    {%- for post in site.posts -%}
      {%- assign date_format = "%Y-%m-%d" -%}
      <div>[ {{ post.date | date: date_format }} ] <a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></div>
    {%- endfor -%}
{%- endif -%}
