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
    {% capture filing_document_type %}
    {% assign dc = _fs[2] | split: '.' %}
    {% case dc[0] %}
        {% when '10q' %}
            10-Q
        {% when '10k' %}
            10-K
        {% when 'ar' %}
            Annual Report
        {% else %}
            {{_fs[2]}}
    {% endcase %}
    {% endcapture %}
    {% assign filing_details = '' | split: ',' | push: _fs[1] | push: _fs[2] | push: formatted_filing_date | push: filing_document_type | push: filing %}
    {% assign _filings = _filings | push: filing_details %}
{% endfor %}

<div style="padding:5% 5% 0 5%;">
    <h2 style="text-align: center;">{{page.title}}</h2>
    <h4>Most Recent Filings</h4>
    {% for f in _filings reversed %}
    <a href="{{ f[4].path }}">{{f[3]}} Document ({{f[2]}}) PDF</a><br /><br />
    {% endfor %}
    <br /><br />
    <a href="xbrl-files.html">XBRL Files</a><br /><br />
    <a href="https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK=0001616741&owner=include&count=40">All SEC Filings</a>
</div>