---
title: "Notes"
permalink: /notes/
---

<p class="example-notice">The entries on this page are example content for the template.</p>

<div class="project-grid">
{% assign entries = site.notes | sort: "date" | reverse %}
{% for entry in entries %}
  <article class="project-card">
    <p class="card-kicker">{{ entry.date | date: "%b %Y" }} · NOTE</p>
    <h2><a href="{{ entry.url | relative_url }}">{{ entry.title }}</a></h2>
    <p>{{ entry.excerpt | strip_html | truncate: 150 }}</p>
    <a class="card-link" href="{{ entry.url | relative_url }}">Read note <span aria-hidden="true">→</span></a>
  </article>
{% endfor %}
</div>
