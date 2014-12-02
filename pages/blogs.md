---
layout: page
permalink: /blogs/
weight: 4
seo_description: Critiques of the literature and science fiction of R.A. Lafferty, with topics including mythology, ancient history, technology, philosophy, magic and mystery.
---


<a href="http://manybooks.net/authors/laffertyr.html">
  <img hspace="30" align="right" title="Project Gutenberg ebooks" src="{{ site.baseurl }}/images/readers.jpg" height="110">
</a>

### Blog Posts

English-language discussion about the literature of R.&nbsp;A.&nbsp;Lafferty. 
<br>
<br>

<div>

<table cellpadding="5">
{% for blog in site.data.blog.entries %}
  <tr>
    <td valign="top"><img hspace="5" vspace="5" src="{{ blog.image }}" width="40"></td>
    <td><a href="{{ blog.link }}">{{ blog.title | truncate: 70, '...' }}</a><br><em>{{ blog.author }}</em></td>
    <td valign="top" width="120" style="color:gray;">{{ blog.date }}</td>
  </tr>
{% endfor %}
</table>

</div>
