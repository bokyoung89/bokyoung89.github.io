---
title: "AWS"
layout: archive
permalink: /AWS/
author_profile: true
sidebar:
    nav: "sidebar-category"
---

{% assign posts = site.categories.AWS %}
{% for post in posts %}
{% include archive-single.html type=entries_layout %}
{% endfor %}