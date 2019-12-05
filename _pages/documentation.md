---
title: "Documentation"
layout: collection
permalink: /docs
excerpt: "Documentation."
last_modified_at: 2019-12-03T21:36:11-04:00
redirect_from:
  - /theme-setup/
toc: true
sidebar:
  nav: "docs"
---

This is the official documentation of the PA JHipster generator.


{% for doc in site.docs %}
## [{{ doc.title }}]({{ doc.permalink }})
  {{ doc.excerpt }}
{% endfor %}
