---
title: Custom Knockout JS binding for jQuery UI Sortable
author: Ivan Zlatev
layout: post
permalink: /2011/06/27/custom-knockout-js-binding-for-jquery-ui-sortable/
aktt_notify_twitter:
  - yes
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 2517
aktt_tweeted:
  - 1
dsq_thread_id:
  - 343724158
categories:
  - Coding
tags:
  - jQuery
  - KnockoutJS
---
For the work in progress Part 3 of my [Editing Variable Length Reorderable Collections in ASP.NET MVC][1] blog series I have created a custom Knockout JS binding that keeps a Knockout JS observable array in sync with a jQuery UI sortable.

It&#8217;s available here:

Basic usage is to use a *sortableList* binding on the *ul* which you want to make a jQuery Sortable and then a *sortableItem* binding for each sortable item (*li*) in the list like that: 

<pre class="brush: xml; title: ; notranslate" title="">&lt;ul data-bind="sortableList: yourObservableArray" &gt; 
    {{each(i, arrayItem) yourObservableArray}} 
        &lt;li data-bind="sortableItem: arrayItem"&gt;&lt;/li&gt; 
    {{/each}} 
&lt;/ul&gt;</pre>

Live example (if you are reading this in a feed reader you will need to open the blog post properly to see this):

<!-- iframe plugin v.3.0 wordpress.org/plugins/iframe/ -->

 [1]: {{ site.url }}/2011/06/16/editing-variable-length-reorderable-collections-in-asp-net-mvc-part-1/