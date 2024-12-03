---
layout: archive
permalink: category-android-java
title: "Android Java"

author_profile: true
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.android-java %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}