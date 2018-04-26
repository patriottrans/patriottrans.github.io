---
layout: default
title: Annual Report and 10K
---

{% assign filings = site.static_files | where_exp: "f","f.path contains 'filings'" %}
{% assign _filings = '' | split: ',' %}
{% for filing in filings %}
    {% if filing.path contains 'ar' or filing.path contains '10k' %}
        {% assign _fs = filing.name | split: '-' %}
        {% capture formatted_filing_date %}{{ _fs[1] | slice: 0,4 }}-{{ _fs[1] | slice: 4,2 }}-{{ _fs[1] | slice: 6,2 }}{% endcapture %}
        {% assign formatted_filing_date = formatted_filing_date | date: "%B %d, %Y" %}
        {% assign dc = _fs[2] | split: '.' %}
        {% assign filing_document_type = '10-K' %}
        {% if dc[0] == 'ar' %}
            {% assign filing_document_type = 'Annual Report' %}
        {% endif %}
        {% assign filing_details = '' | split: ',' | push: _fs[1] | push: _fs[2] | push: formatted_filing_date | push: filing_document_type | push: filing %}
        {% assign _filings = _filings | push: filing_details %}
    {% endif %}
{% endfor %}

## {{page.title}}

{% for f in _filings reversed %}
{% assign year = f[2]|date: "%Y" %}
{% assign name  = f[3] | append: ' ' | append: year | append: ' (' | append: f[2] | append: ') PDF'  %}
{% assign path = f[4].path %}
[{{ name }}]({{ path }})
{% endfor %}