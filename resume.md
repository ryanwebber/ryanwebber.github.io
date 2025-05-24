---
title: Resume
layout: resume
---

# Ryan Webber

[wbbr.ryan@gmail.com](mailto:wbbr.ryan@gmail.com) \| [github.com/ryanwebber](https://github.com/ryanwebber)

---

Bachelor's of Computer Science  @  University of Waterloo

Bachelor's of Business Administration  @  Wilfrid Laurier University

---

{% for gig in site.work %}
## {{ gig.role }}

***{{ gig.time_period }}*** @ **{{ gig.company }}**

{{ gig.content | markdownify }}

{% endfor %}
