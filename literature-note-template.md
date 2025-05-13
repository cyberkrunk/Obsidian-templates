---
tags: {% if allTags %}{{allTags}}{% endif %}
citekey: {{citekey}}
---

<!--
This is an Obsidian template for creating literature notes. Originally based on the template by Alexandra Phelan, it has been recast in a minimalist style. The scripting language is nunjucks.
-->

{{ bibliography }}

{% for type, creators in creators | groupby("creatorType") %}
{% for creator in creators %}
**{{"First" if loop.first}}{{type | capitalize}}**:
{% if creator.name %} {{creator.name}}
{% else %} {{creator.lastName}}, {{creator.firstName}}
{% endif %}
{% endfor %}
{% endfor %}
**Title**: {{title}}
**Year**: {{date | format("YYYY")}}
**Citekey**: {{citekey}} {% if itemType %}
**itemType**: {{itemType}}{% endif %}{% if itemType == "journalArticle" %}
**Journal**: *{{publicationTitle}}* {% endif %}{% if volume %}
**Volume**: {{volume}} {% endif %}{% if issue %}
**Issue**: {{issue}} {% endif %}{% if itemType == "bookSection" %}
**Book**: {{publicationTitle}} {% endif %}{% if publisher %}
**Publisher**: {{publisher}} {% endif %}{% if place %}
**Location**: {{place}} {% endif %}{% if pages %}
**Pages**: {{pages}} {% endif %}{% if DOI %}
**DOI**: {{DOI}} {% endif %}{% if ISBN %}
**ISBN**: {{ISBN}} {% endif %}

{% if abstractNote %}
## Abstract
{{abstractNote}}
{% endif %}

{% if markdownNotes %}
## Notes
{{markdownNotes}}{% endif %}

## Annotations
{% macro calloutHeader(type, color) %}  
{% if type == "highlight" %}  
<mark style="background-color: {{color}}">Quote</mark>  
{% endif %}

{% if type == "text" %}  
Note  
{% endif %}  
{% endmacro %}

{% persist "annotations" %}
{% set newAnnotations = annotations | filterby("date", "dateafter", lastImportDate) %}
{% if newAnnotations.length > 0 %}

### Imported: {{importDate | format("YYYY-MM-DD h:mm a")}}

{% for a in newAnnotations %}
{{calloutHeader(a.type, a.color)}}
{{a.annotatedText}}
{% endfor %}
{% endif %}
{% endpersist %}