---
layout: page
permalink: /works-preview/
weight: 4
---

[//]: # (---------------------------------------------------)
[//]: # ( Copyright 2014, Rich Persaud, All Rights Reserved )
[//]: # (---------------------------------------------------)

<script src="{{ site.baseurl }}/js/store+json2.min.js"></script>
<script type="text/javascript">

// Globals
var u_annote;
var u_config;
var APIver = "20141026_";

function init() 
{
	if (!store.enabled) {
		alert ("Local storage not supported in your browser.  Please disable 'Private Mode' or upgrade to a modern browser.");
		return;
	}

	u_annote = store.get ( APIver + 'userdata' ) || {};
	u_config = store.get ( APIver + 'config' ) || {};

	var keys = [];
	var obj = {};
	for ( var key in u_annote )
	{
		if ( obj = document.getElementById( key ))
			f_star_img ( obj );
	}

	for ( var key in u_config.span )
	{
		display_span (key, u_config.span [key] );
	}

	for ( var key in u_config.subset )
	{
		if ( u_config.subset [key] )
			f_subset (key);
	}
}

function printObj ( testObj )
{
    var szObj       =   ''                                      ;
    for ( var i in testObj )
        szObj       += 'obj[' + i + ']=' + testObj[i]           ;
    return szObj    ;
}

function f_star ( obj )
{
	if ( typeof u_annote[ obj.id ] == 'undefined' )
		u_annote [ obj.id ] = 1;
	else
		u_annote [ obj.id ] += 1;
	
	if ( u_annote [ obj.id ] > 5)
		u_annote [ obj.id ] = 0;

	store.set ( APIver + 'userdata', u_annote );

	f_star_img ( obj );
}

function f_star_img ( obj )
{
	switch ( u_annote [ obj.id ] ) {
		case 1:
			obj.src = "{{ site.baseurl }}/images/star-grey.png";
			break;
		case 2:
			obj.src = "{{ site.baseurl }}/images/star-blue.png";
			break;
		case 3:
			obj.src = "{{ site.baseurl }}/images/star-green.png";
			break;
		case 4:
			obj.src = "{{ site.baseurl }}/images/star-orange.png";
			break;
		case 5:
			obj.src = "{{ site.baseurl }}/images/star-red.png";
			break;
		default:
			obj.src = "{{ site.baseurl }}/images/star-empty.png";
	}
}

function reset_filter ()
{
	var items = document.getElementsByClassName ( 'work-menu' );
	for (i=0; i < items.length; i++)
		items[i].style.textDecoration = 'none';;

	var items = document.getElementsByClassName ( 'work-li' );
	for (i=0; i < items.length; i++)
		items[i].style.display = '';;

	items = document.getElementsByClassName ( 'work-summary' );
	for (i=0; i < items.length; i++)
		items[i].style.display = 'none';;

	if ( typeof u_config.subset == 'undefined' )
		u_config.subset = {};

	for ( var key in u_config.subset )
		u_config.subset [key] = 0;

	store.set ( APIver + 'config' , u_config );
}

function f_subset( className )
{
	var text = document.getElementById( className );
	text.style.textDecoration = 'underline';

	var items = document.getElementsByClassName ( 'work-li' );
	for (i=0; i < items.length; i++)
	{
		if ( ! items[i].classList.contains(  className  ) )
			items[i].style.display = 'none';
	}

	// items = document.getElementsByClassName ( 'work-summary' );
	// for (i=0; i < items.length; i++)
	// {
	// 	items[i].style.display = '';
	// }

	if ( typeof u_config.subset == 'undefined' )
		u_config.subset = {};

	u_config.subset [className] = 1;
	store.set ( APIver + 'config' , u_config );
}

function toggle_story ( node )
{
	var gp = node.parentNode.parentNode; // <li>
	var items = gp.getElementsByClassName ( 'work-summary' );

	for (i=0; i < items.length; i++)
	{
		es = items[i].style;
		es.display = es.display == 'none'? '' : 'none';
	}
}

function toggle_span( className )
{
	var items = document.getElementsByClassName (  className  );
	for (i=0; i < items.length; i++)
	{
			es = items[i].style;
			es.display = es.display == 'none' ? '' : 'none';
	}

	if ( typeof u_config.span == 'undefined' )
		u_config.span = {};

	u_config.span [className] = es.display == 'none' ? 0 : 1;

	var text = document.getElementById( 'menu-' + className);
	text.style.textDecoration = u_config.span [className]? 'underline' : 'none';

	store.set ( APIver + 'config' , u_config );
}

function display_span (className, arg)
{
	var text  = document.getElementById( 'menu-' + className );
	text.style.textDecoration = arg? 'underline' : 'none';

	var items = document.getElementsByClassName ( className );
	for (i=0; i < items.length; i++)
	{
		items[i].style.display = arg? '' : 'none';
	}
}

</script>

<div>
<b>Toggle</b>: 
  <a id="menu-work-award" href="#" onClick="toggle_span('work-award');">Awards</a>
| <a id="menu-work-review" href="#" onClick="toggle_span('work-review');">Reviews</a>
| <a id="menu-work-search" href="#" onClick="toggle_span('work-search');">Search</a>
| <a id="menu-work-annote" href="#" onClick="toggle_span('work-annote');">Annotations</a>
<br>
<br>
<b>Show:</b>
<a id="all" class='work-menu' href="#" onclick="reset_filter();">&nbsp;Index&nbsp;</a>
 | <a id="alt-hist" class='work-menu' href="#" onclick="reset_filter(); f_subset('alt-hist');">Alternate History</a>
 | <a id="tech" class='work-menu' href="#" onclick="reset_filter(); f_subset('tech');">Technology</a>
 | <a id="other" class='work-menu' href="#" onclick="reset_filter(); f_subset('other');">Other</a>
</div>

<br>

<ul style="list-style-type: none;" class="work-ul">
	{% for story in site.data.stories.stories %}
	  	{% for story_details in site.data.consensus.consensus %}
			{% if story.id == story_details.id %}

				<li id="li-{{ story.id }}" class="work-li {{ story_details.class }}">

				<img id="img-{{ story.id }}" style="display:none" class="work-annote" onClick="f_star(this);" src="{{ base.siteurl }}/images/star-empty.png">
				&nbsp;&nbsp;&nbsp;&nbsp;&bull;&nbsp;
				<span class="btitle">
				<a href="#" onclick="toggle_story(this);">
				{{ story_details.title }}
				</a>
				</span>
				({{ story_details.year }})

				<span style="display:none" class="work-search">
				{% if story.isfdb %}
					&middot; <a href="{{ story.isfdb }}">isfdb</a>
				{% endif %}
		
				{% if story.uchronia %}
					&middot; <a href="{{ story.uchronia }}">uchronia</a>
				{% endif %}

				{% if story.search %}
					&middot; <a href="{{ story.search }}">search</a>
				{% endif %}
				</span>

				<span style="display: none" class="work-review">
	  			{% for review in story.reviews %}
					{% for review_details in site.data.reviews.reviews %}
						{% if review.id == review_details.id %}
						&middot; <a href="{{ review_details.url }}">
	  					{% for reviewer in site.data.reviewers.reviewers %}
							{% if reviewer.id == review_details.reviewer %}
								{{ reviewer.name }}
							{% endif %}
	  					{% endfor %}
						</a> ({{ review_details.year }})
						{% endif %}
					{% endfor %}
				{% endfor %}
				</span>

				<span style="display: none" class="work-award">
				{% for award in story_details.awards %}
					{% if forloop.first %}&mdash;<i>{% endif %}
					{{ award.year }} {{ award.award }} {{ award.place }}
					{% if forloop.last %}</i>{% else %},{% endif %}
				{% endfor %}
				</span>

				{% if story_details.summary %}
				<ul style="display:none" class="work-summary"><blockquote>
					{% if story_details.timeline %}({{ story_details.timeline }} A.D.){% endif %}
					"{{ story_details.summary }}"
				</blockquote></ul>
				{% endif %}

			{% endif %}
  		{% endfor %}
	</li>
	{% endfor %}
</ul>


<script type="text/javascript">
	init();
</script>
