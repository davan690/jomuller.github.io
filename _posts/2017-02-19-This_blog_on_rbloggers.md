---
layout: post
title: Find this blog on R-blogger 
categories: 
    - r 
    - blog
---

![R-bloggers logo](/assets/2017-02-19/r-bloggers.png){: .center-image }

[R-bloggers](https://www.r-bloggers.com) is a very popular aggregator of blog about R. I'm very please to annonce this blog to be on also in R-bloggers!

# Why on R-bloggers ?

I'm a daily reader of [R-bloggers](https://www.r-bloggers.com) since years. This is one of main source to update my knowledge about R.
Putting this blog on R-blogger give me extra-viewer, far more comments than before and then it's very stimulating. 

Forthermore, it push me to improve both the content of my posts and the quality of my english!

# How I send posts to R-bloggers?

Only the posts that 

This blog use Jekyll. Then when I decide a post is good enough and reach the [R-blogger criteria](https://www.r-bloggers.com/add-your-blog/), I add it to a dedicated category "r-bloggers". Below, an example of the header of a post I send:

```
---
layout: post
title:  the "ten simple rules for reproducible computational research" are easy to reach for r users
categories: 
    - r 
    - reproductible research
    - r-bloggers
---
```

This add the post to a dedicated [RSS feed](http://blog.jom.link/feed_r-bloggers.xml) for R-bloggers.

To create this RSS feed, configure a [RSS feed file (*feed_r-bloggers.xml*)](https://github.com/jomuller/jomuller.github.io/blob/master/feed_r-bloggers.xml) that filter only on the posts with the `r-bloggers` categories.

```
{% raw %}
  {% if post.categories contains "r-bloggers"%}
{% endraw %}
```

Then, expose this RSS feed to your header (in `_include/head.html`)

```
<link rel="alternate" type="application/rss+xml" title="Joris Muller's blog - r-bloggers" href="{{ "/feed_r-bloggers.xml" | prepend: site.baseurl | prepend: site.url }}" />
```



