---
layout: page
title: Ongoing Analyses
permalink: /analyses/
---

{%- if site.posts.size > 0 -%}
  <ul class="post-list">
    {%- for post in site.posts -%}
      {%- if post.category=="analysis" -%}
        <li>
          <h3>
            <b><a class="post-link" href="{{ post.url | relative_url }}">
                {{ post.title | escape }}
            </a></b>
          </h3>
        </li>
      {%- endif -%}
    {%- endfor -%}
  </ul>
{%- endif -%}
