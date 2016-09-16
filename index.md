---
layout: page
title: Categories
permalink: /
sitemap: false
---

{% assign categories = site.categories | sort %}
{% for category in categories %}
**[{{ category[0] | replace:'-', ' ' }} ({{ category | last | size }})](#{{ category | first | slugify }})**
{% endfor %}

## Documents

{% for category in categories %}
### __{{ category[0] }}__
<a name="{{ category | first | slugify  }}"> </a>
{% assign sorted_posts = site.posts | sort: 'title' %}
{% for post in sorted_posts %}
{%if post.categories contains category[0]%}
### [{{ post.title |  date: "%B %e, %Y" }}]({{ site.url }}{{site.baseurl}}{{ post.url }})
{{ post.excerpt | strip_html | truncate: 160 }}

{%endif%}
{% endfor %}

{% endfor %}