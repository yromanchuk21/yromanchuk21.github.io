---
title: "Research"
permalink: /research/
---

<p class="example-notice">The entries on this page are example content for the template.</p>

<div class="project-grid">
{% assign entries = site.research | sort: "date" | reverse %}
{% for entry in entries %}
  <article class="project-card">
    <p class="card-kicker">{{ entry.date | date: "%Y" }} · RESEARCH</p>
    <h2><a href="{{ entry.url | relative_url }}">{{ entry.title }}</a></h2>
    <p>{{ entry.excerpt | strip_html | truncate: 150 }}</p>
    <a class="card-link" href="{{ entry.url | relative_url }}">Open entry <span aria-hidden="true">→</span></a>
  </article>
{% endfor %}
</div>
