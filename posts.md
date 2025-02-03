---
layout: page
permalink: /posts
title: posts
---

<div class="prompt">
  <span class="promptGreen">gridranger.github.io</span>:<span class="promptBlue">~</span> $ ls -l posts
</div>
<div class="prompt">total {{ site.posts.size }}</div>

{%- if site.posts.size > 0 -%}
    {%- for post in site.posts -%}
      {%- assign date_format = "%Y-%m-%d" -%}
      <div>[ {{ post.date | date: date_format }} ] <a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></div>
    {%- endfor -%}
{%- endif -%}
