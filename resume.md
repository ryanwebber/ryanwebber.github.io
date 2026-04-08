---
title: Resume
layout: resume
---

# Ryan Webber

[webber@apple.com](mailto:webber@apple.com) · [+1 236-808-7692](tel:236-808-7692)

---

Bachelor's of Computer Science  @  University of Waterloo

Bachelor's of Business Administration  @  Wilfrid Laurier University

---

{% for gig in site.work %}
## {{ gig.role }}

***{{ gig.time_period }}*** @ **{{ gig.company }}**

{{ gig.content | markdownify }}

{% endfor %}
