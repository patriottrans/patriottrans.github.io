---
layout: default
title: SEC Filings
---

{% assign filings = site.static_files | where_exp: "f","f.path contains 'filings'" %}
{% assign _filings = '' | split: ',' %}
{% for filing in filings %}
    {% assign _fs = filing.name | split: '-' %}
    {% capture formatted_filing_date %}{{ _fs[1] | slice: 0,4 }}-{{ _fs[1] | slice: 4,2 }}-{{ _fs[1] | slice: 6,2 }}{% endcapture %}
    {% assign formatted_filing_date = formatted_filing_date | date: "%B %d, %Y" %}
    {% assign dc = _fs[2] | split: '.' %}
    {% case dc[0] %}
        {% when '10q' %}
            {% assign filing_document_type = '10-Q' %}
        {% when '10k' %}
            {% assign filing_document_type = '10-K' %}
        {% when 'ar' %}
            {% assign filing_document_type = 'Annual Report' %}
        {% else %}
            {% assign filing_document_type = _fs[2] %}
    {% endcase %}
    {% assign filing_details = '' | split: ',' | push: _fs[1] | push: _fs[2] | push: formatted_filing_date | push: filing_document_type | push: filing %}
    {% assign _filings = _filings | push: filing_details %}
{% endfor %}

## {{page.title}}

#### Most Recent Filings

{% for f in _filings reversed %}
> [{{f[3]}} Document ({{f[2]}}) PDF]({{ f[4].path }})
{% endfor %}

[XBRL Files](xbrl-files.html)

[All SEC Filings](https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK=0001616741&owner=include&count=40)