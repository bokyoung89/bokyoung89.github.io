---
title: "JPA"
layout: archive
permalink: /JPA/
author_profile: true
sidebar:
    nav: "sidebar-category"
---

{% assign posts = site.categories.JPA %}
{% for post in posts %}
{% include archive-single.html type=entries_layout %}
{% endfor %}