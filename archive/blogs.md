---
layout: page
permalink: /blogs/
weight: 4
seo_description: Critiques of the literature and science fiction of R.A. Lafferty, with topics including mythology, ancient history, technology, philosophy, magic and mystery.
---

### Blog Posts

English-language discussion about the literature of R. A. Lafferty. 
<br>
<br>

<div>

<table cellpadding="5">
{% for blog in site.data.blog.entries %}
  <tr>
    <td><img src="{{ blog.image }}" width="40" title="{{ blog.author }}"></td>
    <td><a href="{{ blog.link }}">{{ blog.title }}</a></td>
    <td width="180" style="color:gray;" valign="top">{{ blog.date }}</td>
  </tr>
{% endfor %}
</table>

</div>
