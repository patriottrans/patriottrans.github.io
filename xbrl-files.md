---
layout: default
title: SEC Filings - XBRL Files
---

{% assign fs = site.static_files | where_exp: "f","f.path contains 'xbrl'" %}
{% assign ds = '' | split: ',' %}
{% for f in fs %}
    {% assign d = f.name | slice: 5,8 %}
    {% assign ds = ds | push: d %}
{% endfor %}
{% assign ds = ds | uniq | sort %}
{% assign _ds = '' | split: ',' %}

{% for d in ds reversed %}
    {% capture fd %}{{ d | slice: 0,4 }}-{{ d | slice: 4,2 }}-{{ d | slice: 6,2 }}{% endcapture %}
    {% assign fd = fd | date: "%B %d, %Y" %}
    {% assign _fs = fs | where_exp: "f","f.path contains d" %}
    {% assign _f = '' | split: ',' | push: fd | push: _fs %}
    {% assign _ds = _ds | push: _f %}
{% endfor %}

<h2 style="text-align: center;">{{page.title}}</h2>
{% for d in _ds %}
<h4>{{ d[0] }}</h4>
<ul>
    {% for f in d[1] %}
    <li><a href="{{ f.path }}">{{ f.name }}</a></li>
    {% endfor %}
</ul>
{% endfor %}