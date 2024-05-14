---
title: "Books"
layout: archive
permalink: /Books/
author_profile: true
sidebar:
    nav: "sidebar-category"
---

{% assign posts = site.categories.Books %}
{% for post in posts %}
{% include archive-single.html type=entries_layout %}
{% endfor %}