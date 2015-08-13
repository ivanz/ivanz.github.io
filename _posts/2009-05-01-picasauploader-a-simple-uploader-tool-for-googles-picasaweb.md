---
title: 'PicasaUploader &#8211; a simple uploader tool for Google&#8217;s PicasaWeb'
author: Ivan Zlatev
layout: post
permalink: /2009/05/01/picasauploader-a-simple-uploader-tool-for-googles-picasaweb/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 295
dsq_thread_id:
  - 17061698
categories:
  - Coding
tags:
  - .NET
  - Mono
  - WinForms
---
I finally decided to make the tool I use to upload photos to [Google&#8217;s PicasaWeb][1] online gallery public.

**PicasaUploader** is a simple tool for uploading photos to Google&#8217;s PicasaWeb online gallery. It provides the following functionality:

  * Browse and select an album or create a new one
  * Browse and select/remove photos to upload
  * Duplicate photos handling &#8211; Skip/Replace/Upload
  * Error handling &#8211; if something goes wrong during the upload you will be asked if you want to retry.

The project&#8217;s web page and downloads are here &#8211; [PicasaUploader on the Web][2]. It&#8217;s written in C# and the source code/bug tracker are hosted on GitHub at <http://github.com/ivanz/PicasaUploader>.

<div id='gallery-6' class='gallery galleryid-515 gallery-columns-3 gallery-size-thumbnail'>
  <dl class='gallery-item'>
    <dt class='gallery-icon landscape'>
      <a href='{{ site.url }}/wp-content/uploads/2009/05/albums-screenshot.png'><img width="150" height="150" src="{{ site.url }}/wp-content/uploads/2009/05/albums-screenshot-150x150.png" class="attachment-thumbnail" alt="Album Browser and Album Creation" aria-describedby="gallery-6-516" /></a>
    </dt>
    
    <dd class='wp-caption-text gallery-caption' id='gallery-6-516'>
      Album Browser and Album Creation
    </dd>
  </dl>
  
  <dl class='gallery-item'>
    <dt class='gallery-icon landscape'>
      <a href='{{ site.url }}/wp-content/uploads/2009/05/photos-screenshot.png'><img width="150" height="150" src="{{ site.url }}/wp-content/uploads/2009/05/photos-screenshot-150x150.png" class="attachment-thumbnail" alt="Uploading Photos and Duplicate Photos" aria-describedby="gallery-6-518" /></a>
    </dt>
    
    <dd class='wp-caption-text gallery-caption' id='gallery-6-518'>
      Uploading Photos and Duplicate Photos
    </dd>
  </dl>
  
  <dl class='gallery-item'>
    <dt class='gallery-icon portrait'>
      <a href='{{ site.url }}/wp-content/uploads/2009/05/error-handling-screenshot.png'><img width="150" height="150" src="{{ site.url }}/wp-content/uploads/2009/05/error-handling-screenshot-150x150.png" class="attachment-thumbnail" alt="Error Handling during Upload" aria-describedby="gallery-6-517" /></a>
    </dt>
    
    <dd class='wp-caption-text gallery-caption' id='gallery-6-517'>
      Error Handling during Upload
    </dd>
  </dl>
  
  <br style="clear: both" />
</div>

 [1]: http://picasaweb.google.com
 [2]: {{ site.url }}/projects/picasauploader/