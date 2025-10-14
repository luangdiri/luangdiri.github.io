---
title: "Github Pages - Pagination"
date: 2024-02-09 16:58:56 +08:00
tags:
- tutorial
- programming
---

I've followed Jekyll's [documentation](https://jekyllrb.com/docs/pagination/) on how to implement pagination, but it's not enough for me. Eventually got it figured out after few hours of try and error, so here it is a straightforward guide on how to paginate your blog's homepage using Jekyll's Paginate library.

Firstly, you need to bootstrap your own Jekyll blog using this [guide](https://github.com/chadbaldwin/simple-blog-bootstrap).

Spec from Github Pages [dependency version](https://pages.github.com/versions/) is as follows:
- jekyll 3.9.3
- jekyll-paginate 1.1.0
- minima 2.5.1

This tutorial assumes user to use default configuration of Minima theme. In other words, theme's layout have not been changed.

1. Modify `_config.yml`. Add the following:

```
paginate: 6
paginate_path: "/page/:num/"
```

`paginate`: number of items per page.  
`paginate_path`: can be anything. This will be displayed as the slug in blog's URL.

2. Rename `index.md` to `index.html`. 
3. Add the following code into `index.html`

```html
---
layout: page
---

<div class="home">
  {%- if paginator.posts.size > 0 -%}
    <ul class="post-list">
      {%- for post in paginator.posts -%}
      <li>
        {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
        <span class="post-meta">{{ post.date | date: date_format }}</span>
        <h3>
          <a class="post-link" href="{{ post.url | relative_url }}">
            {{ post.title | escape }}
          </a>
        </h3>
        {%- if site.show_excerpts -%}
          {{ post.excerpt }}
        {%- endif -%}
      </li>
      {%- endfor -%}
    </ul>

    {% if paginator.total_pages > 1 %}
    <div class="pagination">
      {% if paginator.previous_page %}
        <a href="{{ paginator.previous_page_path | relative_url }}">&laquo; Prev</a>
      {% else %}
        <span>&laquo; Prev</span>
      {% endif %}

      {% for page in (1..paginator.total_pages) %}
        {% if page == paginator.page %}
          <em>{{ page }}</em>
        {% elsif page == 1 %}
          <a href="{{ '/' | relative_url }}">{{ page }}</a>
        {% else %}
          <a href="{{ site.paginate_path | relative_url | replace: ':num', page }}">{{ page }}</a>
        {% endif %}
      {% endfor %}

      {% if paginator.next_page %}
        <a href="{{ paginator.next_page_path | relative_url }}">Next &raquo;</a>
      {% else %}
        <span>Next &raquo;</span>
      {% endif %}
    </div>
    {% endif %}

  {%- endif -%}

</div>
```

If you wish to add a banner or a hero image in the front page, simply edit `index.html` and put it above `<div class='home'>` tag. Or modify it as you wish.

See the [demo](https://luangdiri.github.io/).
