---
layout: default
title: "Index"
---

# Ryan Webber

Hello! I'm a software engineer. Here is my [Resume](/resume). Contact me!

## Some things I've built

{% for project in site.projects %}
 * [{{ project.name }}]({{ project.url }}) - {{ project.description }}
{% endfor %}
