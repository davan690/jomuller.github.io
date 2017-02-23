---
layout: post
title: This blog on R-bloggers 
categories: 
    - r 
    - blog
---

![R-bloggers logo](/assets/2017-02-19/r-bloggers.png){: .center-image }

[R-bloggers](https://www.r-bloggers.com) is a very popular aggregator of blogs about R. I'm very pleased to announce this blog to be on also in R-bloggers!

Thanks to [Tal Galili](https://www.r-statistics.com/about/) to have created and maintaining actively this website.

# Why on R-bloggers?

I'm a daily reader of [R-bloggers](https://www.r-bloggers.com) since years. This is one of the main source to update my knowledge about R.
Putting this blog on R-blogger gives me extra-viewer, far more comments than before and then it's very stimulating. 

Furthermore, it pushes me to improve both the content of my posts and the quality of my English!

# How do I send my posts to R-bloggers?

This blog [use Jekyll](http://jekyllrb.com/). Then when I decide a post is good enough and reach the [R-blogger criteria](https://www.r-bloggers.com/add-your-blog/), I add it to a dedicated category "r-bloggers". Below, an example of the header of [a post]({% post_url 2017-01-28-ten_rules_reproductible_research %}) I send:

```
---
layout: post
title:  the "Ten simple rules for reproducible computational research" are easy to reach for r users
categories: 
    - r 
    - reproductible research
    - r-bloggers
---
```

This adds the post to a dedicated [RSS feed](http://blog.jom.link/feed_r-bloggers.xml) for R-bloggers.

To create this RSS feed, configure an [RSS feed file (*feed_r-bloggers.xml*)](https://github.com/jomuller/jomuller.github.io/blob/master/feed_r-bloggers.xml) that filter only on the posts with the `r-bloggers` categories.

{% highlight ruby %}
{% raw %}
{% if post.categories contains "r-bloggers"%}
{% endraw %}
{% endhighlight %}

Then, expose this RSS feed to your header (in `_include/head.html`)

```
<link rel="alternate" type="application/rss+xml" title="Joris Muller's blog - r-bloggers" href="{{ "/feed_r-bloggers.xml" | prepend: site.baseurl | prepend: site.url }}" />
```



