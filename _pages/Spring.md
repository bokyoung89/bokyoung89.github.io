---
title: "Spring"
layout: archive
permalink: /Spring/
author_profile: true
sidebar:
    nav: "sidebar-category"
---

{% assign posts = site.categories.Spring %}
{% for post in posts %}
{% include archive-single.html type=entries_layout %}
{% endfor %}