---
layout: page
permalink: /publications/
title: Publications
description: Publications about some works that I have done or collaborated with. You can download the documents to read them in full.
sections:
  - bibquery: "@article"
    text: "Journal articles"
  - bibquery: "@inproceedings"
    text: "Conference and workshop papers"
  - bibquery: "@misc|@phdthesis|@mastersthesis"
    text: "Miscellaneous"
social: true
nav: true
nav_order: 4
---

<div class="publications">

{%- assign years = "" -%}

{%- for section in page.sections %}
  <a id="{{section.text}}"></a>
  <p class="bibtitle">{{section.text}}</p>

  {%- bibliography -f {{site.scholar.bibliography}} -q {{section.bibquery}} -o year -l 0 | split: ", " | uniq | sort_natural | reverse | join: ", " | strip_newlines | assign: years %}
  
  {%- for y in years | split: ", " %}

    {%- comment -%} Count bibliography in actual section and year {%- endcomment -%}
    {%- capture citecount -%}
    {%- bibliography_count -f {{site.scholar.bibliography}} -q {{section.bibquery}}[year={{y}}] -%}
    {%- endcapture -%}

    {%- comment -%} If exist bibliography in actual section and year, print {%- endcomment -%}
    {%- if citecount != "0" %}

      <h2 class="year">{{y}}</h2>
      {% bibliography -f {{site.scholar.bibliography}} -q {{section.bibquery}}[year={{y}}] %}

    {%- endif -%}

  {%- endfor %}

{%- endfor %}

</div>
