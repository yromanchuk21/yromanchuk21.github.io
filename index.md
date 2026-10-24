---
layout: home
author_profile: false
classes: wide
---

<section class="terminal-hero">
  <p class="eyebrow">SYSTEMS / SECURITY / ENGINEERING</p>
  <h1>Build notes from the edge of the stack.</h1>
  <p class="hero-copy">A focused portfolio for embedded hardware, defensive security research, and the practical details in between.</p>
  <div class="hero-actions">
    <a class="btn btn--primary" href="#projects">Explore projects</a>
    <a class="btn btn--inverse" href="{{ '/about/' | relative_url }}">About this work</a>
  </div>
  <p class="example-notice">Template content below is illustrative. Replace it with your own work, evidence, and links.</p>
</section>

<section id="projects" class="portfolio-section">
  <div class="section-heading">
    <p class="eyebrow">SELECTED WORK</p>
    <h2>Projects</h2>
  </div>
  <div class="project-grid">
  {% assign featured_projects = site.projects | where: "featured", true | sort: "order" %}
  {% for project in featured_projects %}
    <article class="project-card">
      <p class="card-kicker">{{ project.category | default: "PROJECT" }}</p>
      <h3><a href="{{ project.url | relative_url }}">{{ project.title }}</a></h3>
      <p>{{ project.excerpt | strip_html | strip_newlines | truncate: 145 }}</p>
      <ul class="tag-list">
      {% for tag in project.tags %}<li>{{ tag }}</li>{% endfor %}
      </ul>
      <a class="card-link" href="{{ project.url | relative_url }}">Read case study <span aria-hidden="true">→</span></a>
    </article>
  {% endfor %}
  </div>
</section>

<section class="portfolio-section split-section">
  <div>
    <p class="eyebrow">IN PROGRESS</p>
    <h2>Research</h2>
    <p>Methods, lab notes, and longer-form technical explorations—organized to show reasoning as well as results.</p>
    <a class="text-link" href="{{ '/research/' | relative_url }}">Browse research <span aria-hidden="true">→</span></a>
  </div>
  <div>
    <p class="eyebrow">FIELD NOTES</p>
    <h2>Notes</h2>
    <p>Concise write-ups on tooling, embedded debugging, industrial systems, and secure engineering practice.</p>
    <a class="text-link" href="{{ '/notes/' | relative_url }}">Read notes <span aria-hidden="true">→</span></a>
  </div>
</section>
