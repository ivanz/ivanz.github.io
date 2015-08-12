---
title: 'Batch renaming files in the form of &#8220;prefixNUMBERsuffix&#8221;'
author: Ivan Zlatev
layout: post
permalink: /2006/05/27/batch-renaming-files-with-incrementing-number/
views:
  - 12
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1432717
categories:
  - Coding
---
I finally managed to complete the bash script I wanted to have. I wanted to batch rename files in the form of &#8220;prefixNUMBERsuffix&#8221;. E.g asd.jpg, dfe.jpg to cats\_1.jpg, cats\_2.jpg ,etc. And yeah.. that is for my gallery in the blog. So here it goes&#8230;

<pre lang="bash">#!/bin/bash 

if (($# != 4));
    then
    echo "Usage: pcrename [prefix] [first_number] [suffix] "[mask]""
    echo "Example: pcrename fun_ 1 .jpg "*.jpg""
else
    prefix=$1
    i=$2
    suffix=$3
    mask=$4
    
    for fname in $mask
	  do
	  mv "${fname}" "${prefix}${i}${suffix}"
	  ((i++))
	done
fi</pre>

P.S My conclusion is that I don&#8217;t like bash scripting at all. It hurts my brain!!!