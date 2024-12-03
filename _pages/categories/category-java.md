---
layout: archive
permalink: category-java
title: "Java"

author_profile: true
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.Java %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}