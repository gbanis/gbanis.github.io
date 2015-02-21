---
layout:     post
title:      "How to Add Google Authorship to Jekyll"
description:  "A step-by-step process to help you add Google Authorship to your Jekyll blog."
date:       2014-12-09 12:00:00
tags:
  - google authorship
  - jekyll
  - seo
author:     George Banis
---

## TL;DR

Adding Google Authorship to Jekyll will help you with SEO and will also connect your name with your blog post on search results.

## The Process for Adding Google Authorship to Jekyll

Before you implement the markup on your Jekyll site, make sure:

- You have a Google+ page
- You have linked your website to your page
- You have added your website to Google Webmaster Tools

Once you are done with that you'll need to:

1. Create an `authors.yml` file to store your author data
2. Set the author in your post
3. Update your `post_meta.html` partial with the Google+ markup

### 1. Create an authors.yml file

We'll need this file to store the name and Google+ url of the author. This is a great way to have it set up, especially if you plan on having multiple authors.

- Create a `_data` folder in your root directory (if you don't already have one).
- Create an `authors.yml` file in the `_data` folder.
- Enter your author info in that file.

For example, my information is the following:

```yaml
# _data/authors.yml
- name: George Banis
  google_plus: https://plus.google.com/+GeorgeBanis

```

### 2. Set the author in your post

You'll need to add the author name in your post so that you'll be able to find the right author for your post with liquid.

Here's an example of what this post's markdown looks like:

```
---
layout:     post
title:      "How to Add Google Authorship to Jelyll"
date:       2014-12-09 12:00:00
tags:
  - google authorship
  - jekyll
  - seo
author:     George Banis  # <--- this is what you need to add
---

## TL;DR

Adding Google Authorship to Jekyll will help you with SEO and will also connect your name with your blog post on search results.
[...]

```

### 3. Update your post_meta.html partial

This is where all the magic happens. Once you've set up your data correctly, you should be able to access the author information by matching the `page.author` or `post.author` with the `site.data.authors`.

Here's a simple version of the code you'll need to do that:

```html
{%raw%}
{% for author in site.data.authors %}
  {% if author.name == page.author %} <!-- or post.author -->
    by <span itemscope itemtype="http://schema.org/Person">
      <a href="{{author.google_plus}}" rel="publisher" itemprop="name">
        {{author.name}}
      </a>
    </span>
  {% endif %}
{% endfor %}
{%endraw%}
```

And in case you are wondering, here's what my custom `post_meta.html` looks like:

```html
<!-- _includes/post_meta.html -->
{%raw%}
{% if page.author %}
<!-- Post meta tags for individual post page -->
<div class="post-meta">
  <span class="post-date">{{ page.date | date: "%b %-d, %Y" }}</span>{% for author in site.data.authors %}{% if author.name == page.author %} • by <span class="post-author" itemscope itemtype="http://schema.org/Person"> <a href="{{author.google_plus}}" rel="publisher" itemprop="name">{{author.name}}</a> </span>{% endif %}{% endfor %}
  {% if page.meta %} • {{ page.meta }}</span>{% endif %}
  <p class="post-tags"><i class="fa fa-tags"></i> {% for tag in page.tags %} <a href="/tags/{{ tag }}" title="View posts tagged with &quot;{{ tag }}&quot;"><u>{{ tag }}</u></a>{% if forloop.last != true %}, {% endif %} {% endfor %}</p>
</div>
{% endif %}

{% if post.author %}
<!-- Post meta tags for index page -->
<div class="post-meta">
  <span class="post-date">{{ post.date | date: "%b %-d, %Y" }}</span>{% for author in site.data.authors %}{% if author.name == post.author %} • by <span class="post-author" itemscope itemtype="http://schema.org/Person"> <a href="{{author.google_plus}}" rel="publisher" itemprop="name">{{author.name}}</a> </span>{% endif %}{% endfor %}
{{post.meta}}
  {% if post.meta %} • {{ post.meta }}</span>{% endif %}
  <p class="post-tags"><i class="fa fa-tags"></i> {% for tag in post.tags %} <a href="/tags/{{ tag }}" title="View posts tagged with &quot;{{ tag }}&quot;">{{ tag }}</a>{% if forloop.last != true %}, {% endif %}{% endfor %}</p>
</div>
{% endif %}
{%endraw%}
```

Hope this helps!

Let me know what you think with a quick comment.
