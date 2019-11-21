---
layout: page
title: Blog Posts
permalink: /posts/
---


{%- if site.posts.size > 0 -%}
  <ul class="post-list">
    {%- for post in site.posts -%}
      {%- if post.category=="post" -%}
        <li>
          {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
          <span class="post-meta">{{ post.date | date: date_format }}</span>
          <h3>
            <b><a class="post-link" href="{{ post.url | relative_url }}">
                {{ post.title | escape }}
            </a></b>
            <i><font size="4">
              {{ post.subtitle }}
            </font></i>
          </h3>
        </li>
      {%- endif -%}
    {%- endfor -%}
  </ul>

{%- endif -%}
