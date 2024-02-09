---
title: "Github Pages - Implementing Pagination"
date: 2024-02-09 16:58:56 +08:00
tags:
- tutorial
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

<script src="https://gist.github.com/luangdiri/eaac428106aff1e574bc0555995d757d.js"></script>

If you wish to add a banner or a hero image in the front page, simply edit `index.html` and put it above `<div class='home'>` tag. Or modify it as you wish.

See the [demo](https://luangdiri.github.io/).
