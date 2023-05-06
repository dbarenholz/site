---
permalink: /code/
title: Code
toc: false
---

See my coding projects below.

<div class="grid flex">
  {% for note in site.notes %}
    {% if note.tag == 'code' %}
     <div class="notes-item {{ note.tag }}">
        <a class="notes-link" data-toggle="modal" href="{{ site.url }}/{{ note.url }}">
          <h4>{{ note.title }}</h4>
        </a>
        <p> {{ note.excerpt | markdownify }}</p>
     </div>
     {% endif %}
  {% endfor %}
</div>

<style>
    /* Styles for grid elements */
    .flex {
        display: flex;
        flex-wrap: wrap;
        justify-content: flex-start;
        align-content: flex-start;
        flex-direction: row;
        align-items: flex-start;
        padding-left: 1em;
    }
    .notes-item {
        max-width: 12em;
    }
</style>