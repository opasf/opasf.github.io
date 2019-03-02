---
layout: page
title: Documentation
permalink: /docs/
---

<ul>
    {% for doc in site.documentation %}
        <li><a href="{{ doc.permalink }}">{{ doc.title }}</a></li>
    {% endfor %}
</ul>