---
layout: page
title: Projects
permalink: /projects/
description: A growing collection of your cool projects.
nav: true
nav_order: 1
display_categories: [astronomy, automotive, education, powerlifting]
horizontal: false
---

<!-- pages/projects.md -->
<style>
/* Outline layout: each category badge sits in a left gutter beside its own group. */
.projects {
  /* Let the badge gutter hang into the page margin so the cards keep their width.
     Collapses to 0 once the viewport is no wider than the content column. */
  --outdent: max(0px, min(160px, calc((100vw - var(--max-content-width, 930px)) / 2 - 24px)));
  margin-left: calc(-1 * var(--outdent));
  width: calc(100% + var(--outdent));
}
.category-block {
  display: flex;
  gap: 1.5rem;
  --badge-w: 256px;
}
.category-badge {
  flex: 0 0 var(--badge-w);
  width: var(--badge-w);
}
.category-badge .badge-inner {
  position: sticky;
  top: 5.5rem;
  padding-top: 2.4rem; /* lines the badge up with the category rule */
}
.category-badge img {
  width: 100%;
  height: auto;
  opacity: 0.85;
}
/* The badges are transparent black line art — flip them white in dark mode. */
html[data-theme="dark"] .category-badge img { filter: invert(1); }

.category-body {
  flex: 1 1 auto;
  min-width: 0;
}

/* Card grid, sized here rather than with row-cols-* so it accounts for the gutter. */
.project-grid > * { flex: 0 0 100%; max-width: 100%; }
@media (min-width: 820px) {
  .project-grid > * { flex: 0 0 50%; max-width: 50%; }
}
@media (min-width: 1250px) {
  .project-grid > * { flex: 0 0 33.3333%; max-width: 33.3333%; }
}

@media (max-width: 819.98px) {
  .category-block { gap: 1rem; --badge-w: min(120px, 26vw); }
  .category-badge .badge-inner { padding-top: 2rem; }
}
</style>

<div class="projects">
{% if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized projects, each beside its badge -->
  {% for category in page.display_categories %}
  <section class="category-block">
    <div class="category-badge">
      <div class="badge-inner">
        <img src="{{ '/assets/img/logos/' | append: category | append: '.png' | relative_url }}" alt="Platypus {{ category }} badge" loading="lazy" />
      </div>
    </div>
    <div class="category-body">
      <a id="{{ category }}" href=".#{{ category }}">
        <h2 class="category">{{ category }}</h2>
      </a>
      {% assign categorized_projects = site.projects | where: "category", category %}
      {% assign sorted_projects = categorized_projects | sort: "importance" %}
      <!-- Generate cards for each project -->
      {% if page.horizontal %}
      <div class="container">
        <div class="row row-cols-1 row-cols-md-2">
        {% for project in sorted_projects %}
          {% include projects_horizontal.liquid %}
        {% endfor %}
        </div>
      </div>
      {% else %}
      <div class="row project-grid">
        {% for project in sorted_projects %}
          {% include projects.liquid %}
        {% endfor %}
      </div>
      {% endif %}
    </div>
  </section>
  {% endfor %}

{% else %}

<!-- Display projects without categories -->

{% assign sorted_projects = site.projects | sort: "importance" %}

  <!-- Generate cards for each project -->

{% if page.horizontal %}

  <div class="container">
    <div class="row row-cols-1 row-cols-md-2">
    {% for project in sorted_projects %}
      {% include projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
{% endif %}
</div>
