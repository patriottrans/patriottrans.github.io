---
layout: default
title: Corporate Governance Documents
---

## {{page.title}}

{% assign fs = site.static_files | where_exp: "f","f.path contains 'legal'" %}
{% for f in fs %}
{% assign g = f.name | split: '-' %}
[{{g[1]}}]({{ f.path }})
{% endfor %}