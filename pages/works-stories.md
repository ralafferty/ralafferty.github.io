---
layout: page
permalink: /works/stories/
title: 
weight: 4
redirect_from: "/works-preview/"
redirect_from: "/test/works-preview/"
---

[//]: # (---------------------------------------------------)
[//]: # ( Copyright 2014, Rich Persaud, All Rights Reserved )
[//]: # (---------------------------------------------------)

<script src="{{ site.baseurl }}/js/store+json2.min.js"></script>
<script type="text/javascript">

/* Globals */
var u_annote;
var u_config;
var APIver = "20141026_";  /* any change to this value requires migration of client data */
var j_stories;
var sz_headline = "";
var a_tags = {};

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
		if ( obj = document.getElementById( key ))
			f_star_img ( obj );

	for ( var key in u_config.span )
		display_span (key, u_config.span [key] );

	var menu = document.getElementById( 'alltags' );
	var items = menu.getElementsByClassName ( 'work-menu' );
	for (i=0; i < items.length; i++)
		a_tags [ items[i].id ] = items[i].innerHTML;

	var subset_found = false;
	for ( var key in u_config.subset )
	{
		if ( u_config.subset [key] )
		{
			f_subset (key);
			subset_found = true;	
			break;
		}
	}

	if (!subset_found)
		reset_filter();
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


function fast_class_swap ()
{
	var elem = document.getElementsByTagName('div')[0];
	elem.className = elem.className.replace('otherClass', 'newClass');
}

function reset_filter ()
{
	/* var items = document.getElementsByClassName ( 'work-menu' );
	for (i=0; i < items.length; i++)
		items[i].style.textDecoration = 'none'; */

	var ulist = document.getElementById( 'work-ul' );
	var items = ulist.getElementsByClassName ( 'work-li' );
	for (i=0; i < items.length; i++)
	{
		var el =  items[i];
		var tagspan = el.getElementsByClassName ( 'work-tags' );
		var init = 0;

		for (var j=0; j < el.classList.length; j++)
		{
			var testClass = el.classList.item(j);
			if ( testClass != 'work-li') 
			{
				if (!init)
				{
					init = 1;
					tagspan[0].innerHTML = '<i style="color:#ADADAD;" class="fa fa-tag"></i> &nbsp;';
				}
				
				tagspan[0].innerHTML += '<a id="' + testClass + '" href="javascript:void(0);" onclick="f_subset(' + "'" + testClass + "'" + ');" class="work-menu">' + a_tags [ testClass ] + '</a> ';
			}
		}
		el.style.display = '';
	}

	minimize_summary (); 

	document.getElementById("headline").innerHTML = 'All Stories';

	if ( typeof u_config.subset == 'undefined' )
		u_config.subset = {};

	for ( var key in u_config.subset )
		u_config.subset [key] = 0;

	store.set ( APIver + 'config' , u_config );
	return false;
}

function minimize_summary ()
{
	items = document.getElementsByClassName ( 'work-summary' );
	for (i=0; i < items.length; i++)
		items[i].style.display = 'none';;
}

function log(param){
    setTimeout(function(){
        throw new Error("Debug: "+param)
    },0)
}

function f_subset( className )
{
	minimize_summary (); 

	var text = document.getElementById( className );

	var ulist = document.getElementById( 'work-ul' );
	var items = ulist.getElementsByClassName ( 'work-li' );
	for (i=0; i < items.length; i++)
	{
		var el =  items[i];
		if ( ! el.classList.contains(  className  ) )
			el.style.display = 'none';
		else
		{
			var tagspan = el.getElementsByClassName ( 'work-tags' );
			var init = 0;

			for (var j=0; j < el.classList.length; j++)
			{
				var testClass = el.classList.item(j);
				if ( testClass != 'work-li' && testClass != className )
				{
					if (!init)
					{
						init = 1;
						tagspan[0].innerHTML = '<i style="color:#ADADAD;" class="fa fa-tag"></i> &nbsp;';
					}
					tagspan[0].innerHTML += '<a id="' + testClass + '" href="javascript:void(0);" onclick="f_subset(' + "'" + testClass + "'" + ');" class="work-menu">' + a_tags [ testClass ] + '</a> ';
				}
			}
			el.style.display = '';
		}
	}

	document.getElementById("headline").innerHTML = text.text;

	if ( typeof u_config.subset == 'undefined' )
		u_config.subset = {};

	u_config.subset [className] = 1;
	store.set ( APIver + 'config' , u_config );
	return false;
}

function toggle_story ( node )
{
	var gp = node.parentNode.parentNode; /* <li> */
	var items = gp.getElementsByClassName ( 'work-summary' );

	for (i=0; i < items.length; i++)
	{
		es = items[i].style;
		es.display = es.display == 'none'? '' : 'none';
	}
	return false;
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
	text.classList.toggle('pure-button-active');

	store.set ( APIver + 'config' , u_config );
	return false;
}

function display_span (className, arg)
{
	var text  = document.getElementById( 'menu-' + className );

	if ( !text )
		return false;
	if (arg)
		text.classList.add('pure-button-active');
	else
		text.classList.remove('pure-button-active');

	var items = document.getElementsByClassName ( className );
	for (i=0; i < items.length; i++)
	{
		items[i].style.display = arg? '' : 'none';
	}
	return false;
}

/*
function f_comments (storyid)
{
	var idName = 'd-story-' + storyid;

	var items = document.getElementsByClassName ( 'd-cmnt' );
	for (i=0; i < items.length; i++)
	{
		var el =  items[i];

		if (el.id == idName)
			el.innerHTML = '<div id="disqus_thread"></div>';
		else
			el.innerHTML = '';
	}
	disqus_identifier = idName;
	loadDisqus();
	return false;
}

function loadJSON(path, success, error)
{
    var xhr = new XMLHttpRequest();
    xhr.onreadystatechange = function()
    {
        if (xhr.readyState === XMLHttpRequest.DONE) {
            if (xhr.status === 200) {
                if (success)
                    success(JSON.parse(xhr.responseText));
            } else {
                if (error)
                    error(xhr);
            }
        }
    };
    xhr.open("GET", path, true);
    xhr.send();
}

// Register the handler for any event we might receive
if (document.addEventListener) {
        document.addEventListener("DOMContentLoaded", init, false);
        document.addEventListener("readystatechange", init, false);
        window.addEventListener("load", init, false);
}
else if (document.attachEvent) {
        document.attachEvent("onreadystatechange", init);
        window.attachEvent("onload", init);
}

loadJSON('{{ site.baseurl }}/archive/test-json.txt', 
         function(data) { j_stories = data; alert (printObj(data.stories[0])); },
         function(xhr) { console.error(xhr); } 
); 

*/

</script>

## The Short Stories of R.A. Lafferty

<div>
<b>Toggle On/Off:&nbsp;</b>
<button class="button-small pure-button" id="menu-work-tags" onClick="toggle_span('work-tags');"><i style="color:#ADADAD;" class="fa fa-tag"></i> Tags</button>
&nbsp;&nbsp;
<button class="button-small pure-button" id="menu-work-review" href="javascript:void(0);" onClick="toggle_span('work-review');"><i style="color:#ADADAD;" class="fa fa-thumbs-up"></i> Reviews</button>
&nbsp;&nbsp;
<button class="button-small pure-button" id="menu-work-annote" href="javascript:void(0);" onClick="toggle_span('work-annote');"><i style="color:#ADADAD;" class="fa fa-star"></i> Star</button>

<br>
<br>
<b>Filter by Tag:</b>
<a id="all" class='work-menu' href="javascript:void(0);" onclick="reset_filter();">&nbsp;ALL STORIES&nbsp;</a> 

<span id="alltags">
{% for tag in site.data.reviewer-tags.tag %}
	{% for definition in tag.definition %}
	&nbsp;<a id="{{ definition.id }}" href="javascript:void(0);" onclick="f_subset('{{ definition.id }}');"  
		class='work-menu' title="{{ definition.detail }}">&middot;&nbsp;{{ definition.title }}</a>
	{% endfor %}
{% endfor %}
</span>

</div>

<h2><span id="headline">All Stories</span></h2>

<ul style="list-style-type: none;" id="work-ul" class="work-ul">
	{% for story in site.data.stories.stories %}
	  	{% for story_details in site.data.consensus.consensus %}

			{% if story.id == story_details.id %}
				<li id="li-{{ story.id }}" class="work-li 

					{% for tag in site.data.reviewer-tags.tag %}
						{% for map in tag.map %}
							{% if story.id == map.id %}
								{{ map.tags }}
							{% endif %}
						{% endfor %}
					{% endfor %}
				">

				&nbsp;&nbsp;
				<span class="btitle">
					{% if story_details.excerpt %}
						<a href="javascript:void(0);" onclick="toggle_story(this);">
					{% endif %}

					{{ story_details.title }}

					{% if story_details.excerpt %}
						</a>
					{% endif %}
				</span>

				{% if story.isfdb %}
					(<a href="{{ story.isfdb }}">{{ story_details.year }}</a>)
				{% else %}
					({{ story_details.year }})
				{% endif %}

				&nbsp;&nbsp;&nbsp;&nbsp;
				<img id="img-{{ story.id }}" style="display:none" class="work-annote" onClick="f_star(this);" src="{{ base.siteurl }}/images/star-empty.png">

	  			{% for review in story.reviews %}
					{% for review_details in site.data.reviews.reviews %}
						{% if review.id == review_details.id %}
							{% if review_details.summary %}
								<ul style="display: none;" class="work-review taglist">
								<em>{{ review_details.summary }}
								<br>&nbsp;&nbsp;&nbsp;
								{{ review_details.rating }}
								&mdash;
								{% for reviewer in site.data.reviewers.reviewers %}
									{% if reviewer.id == review_details.reviewer %}
										{{ reviewer.name }}, {{ review_details.year }}
									{% endif %}
								{% endfor %}
								</em></ul>
							{% endif %}

						{% endif %}
					{% endfor %}
				{% endfor %}

				{% if story_details.excerpt %}
				<ul style="display:none" class="work-summary"><blockquote>
					{% if story_details.timeline %}({{ story_details.timeline }} A.D.){% endif %}
					"{{ story_details.excerpt }}" &nbsp;&mdash;R. A. Lafferty
				</blockquote></ul>
				{% endif %}

				<ul class="work-award taglist">
				{% for award in story_details.awards %}
					{% if forloop.first %} 
					<i style="color:#ADADAD;" class="fa fa-trophy"></i> &nbsp;
					{% endif %}
					&middot; {{ award.year }} {{ award.award }} {{ award.place }}
					{% if forloop.last %}{% else %}&nbsp;&nbsp;{% endif %}
				{% endfor %}
				</ul>

				<ul class="work-review taglist">

	  			{% for review in story.reviews %}
						{% if forloop.first %} 
							{% assign hdr = '1' %}
						{% endif %}
					{% for review_details in site.data.reviews.reviews %}

						{% if review.id == review_details.id %}
						{% if review_details.url %}

						{% if hdr == '1' %}
							<i style="color:#ADADAD;" class="fa fa-thumbs-up"></i> &nbsp;
							{% assign hdr = '0' %}
						{% endif %}

						&middot; <a href="{{ review_details.url }}">
	  					{% for reviewer in site.data.reviewers.reviewers %}
							{% if reviewer.id == review_details.reviewer %}
								{{ reviewer.name }}
							{% endif %}
	  					{% endfor %}
						</a> ({{ review_details.year }}) &nbsp;
						{% endif %}
						{% endif %}
					{% endfor %}
				{% endfor %}
				</ul>

				<ul class="taglist" style="list-style-type: none;">
					<li id="work-tags" class="work-tags"></li>
					<li><i style="color:#ADADAD;" class="fa fa-comment"></i> 
					&nbsp; &middot; <a class="disqus-comment-count" data-disqus-identifier="d-story-{{ story.id }}" href="{{ site.baseurl }}/works/stories/{{ story_details.url }}">Comments</a>
					</li>
				</ul>

	
			{% break %}
			{% endif %}
  		{% endfor %}
	</li>
	{% endfor %}
</ul>


<script type="text/javascript">
init(); 

var disqus_shortname = '{{ site.disqus_shortname }}';
var disqus_identifier = '';
var disqus_url = '';

/* * * DON'T EDIT BELOW THIS LINE * * */
(function () {
var s = document.createElement('script'); s.async = true; s.type = 'text/javascript';
s.src = 'http://' + disqus_shortname + '.disqus.com/count.js';
(document.getElementsByTagName('HEAD')[0] || document.getElementsByTagName('BODY')[0]).appendChild(s);
}());

</script>
<noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
