---
layout: page
title: Penyelenggaraan Sebelumnya
---

{% comment %} pop to remove default "posts" collection {% endcomment %}
{% assign sorted_collections = site.collections | sort: 'label' | pop | reverse %}
<ul>
{% for collection in sorted_collections %}
    {% if forloop.index == 1 %}
        <li><a href="/">{{ collection.label }}</a> (saat ini)</li>
    {% else %}
        <li><a href="/{{ collection.label }}/">{{ collection.label }}</a></li>
    {% endif %}
{% endfor %}
</ul>

Setiap tahun, kuliah bersifat mandiri. Kami merekomendasikan memulai dari versi materi yang paling baru. Ada variasi topik yang dibahas setiap tahun, sehingga kami tetap menyediakan catatan dan video untuk versi sebelumnya dari kursus ini.
