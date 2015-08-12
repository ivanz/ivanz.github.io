---
title: 'PicasaUploader 0.5: Upload videos'
author: Ivan Zlatev
layout: post
permalink: /2010/06/05/picasauploader-0-5-upload-videos/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 112
dsq_thread_id:
  - 104599236
categories:
  - Technology
tags:
  - .NET
  - Open Source
  - PicasaWeb
---
I have released version 0.5 of [PicasaUploader ][1]&#8211; the simple PicasaWeb uploader tool. Changes:

  * Add support for uploading videos. 
      * Supported files are: .avi, .mpeg, .mpg, .wmv, .3gp, .asf, .mp4, .mov. However PicasaWeb can still reject any of those if it doesn&#8217;t like it. Note also that videos won&#8217;t be playable in the album immediately because they have to be postprocessed by PicasaWeb.
  * File size limits: 20MB for photo files and 100MB for video files.

Get it [here][1].

<div id="attachment_719" style="width: 310px" class="wp-caption aligncenter">
  <a href="http://ivanz.com/wp-content/uploads/2009/04/video-upload1.png"><img class="size-medium wp-image-719" title="Video Upload" src="http://ivanz.com/wp-content/uploads/2009/04/video-upload1-300x221.png" alt="Video Upload" width="300" height="221" /></a>
  
  <p class="wp-caption-text">
    Video Upload
  </p>
</div>

 [1]: http://ivanz.com/projects/picasauploader/