---
layout: page
permalink: /biography/timeline/
weight: 4
title: Timeline
seo_description: Timeline of significant events in the life and work of American writer R.A. Lafferty, winner of science-fiction and fantasy awards.
---

<div>

<table cellpadding="7">
  <tr>
    <td><b>Date</b></td>
    <td><b>Age</b></td>
    <td><b>Event</b></td>
    <td><b>Location</b></td>
  </tr>
{% for event in site.data.timeline.timeline %}
  <tr>
    <td valign="top" style="color:gray;">{{ event.year }}&nbsp;{{ event.month | truncate: 3, '' }}<a name="{{ event.year }}"></a></td>
    <td valign="top"><em>{{ event.age }}</em></td>
    <td>{{ event.title }}</td>
    <td valign="top" style="color:gray;">{{ event.geography }}</td>
  </tr>
{% endfor %}
</table>

</div>
