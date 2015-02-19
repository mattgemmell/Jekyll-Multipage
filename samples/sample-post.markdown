---
layout: post
title: "Sample Multipage Post"
comments: false
description: "Just testing the multipage plugin."
date: 2015-02-19 09:00
categories:
- testing
multipage: true
---


This is page one.

Some content goes here.

<!--more-->

A bit more content, and we'll link to [a later section](#sample-heading).


{% page_break %}


{% if page.multipage.is_collated_page %}
This is the collated page!
{% else %}
This is page **{{ page.multipage.page_number }}**.
{% endif %}

{% page_break %}


This is another page. Welcome to {{site.title}}, by the way.

Here's a manually-inserted paging bar:

{% paging %}

And a little bit more content.


{% page_break %}


Yet another page. How about a heading?

##Sample Heading

I think that'll just about do for now. The next page is the last one.


{% page_break %}


And here we are at the final page. Don't forget to visit the [collated page]({{ page.multipage.collated_path }}), too.
