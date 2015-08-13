---
title: 'dev4good &#8211; Developing for a Better World'
author: Ivan Zlatev
layout: post
permalink: /2011/07/04/dev4good-developing-for-a-better-world/
aktt_notify_twitter:
  - yes
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 455
aktt_tweeted:
  - 1
dsq_thread_id:
  - 349618076
categories:
  - Coding
  - Diary
tags:
  - Achievements
  - Charity
  - Open Source
---
[  
][1]Last weekend (2-3 July) I attended the first ever [dev4good][2] &#8211; a charity hackathon where developers try to solve charity problems in a weekend. It took place in Hammersmith, London in a amazing venue on the riverside:

[<img class="aligncenter size-large wp-image-945" title="IMG_1305" src="{{ site.url }}/wp-content/uploads/2011/07/IMG_1305-1024x764.jpg" alt="" width="620" height="462" />][3]

Three charities came to use with the following problems:

[Hope and Play][4] were looking for a way to locate and manage resources. And allow other charities or people to do the same.

[The Ministry of Stories][5] were looking for a way to get more young people writing stories and publishing these to the world. All while helping to generate a little more cash along the way.

[The Design and Craft Council][6] want to be able to automate their awards processes.

We were about 25 people and were able to split in three teams. Here is a video of my team explaining what we are planning to achieve for the Hope and Play project (I am the one speaking first):



Basically we wanted to create a proof of concept system that:

<ol id="internal-source-marker_0.27231994923204184">
  <li>
    Provides a central web portal to aggregate information and also allow charities to communicate what projects they are running, and in which locations.
  </li>
  <li>
    Allows charities to communicate in real-time work and achievements on their projects via the web portal, Twitter or mobile text messages (SMS).
  </li>
  <li>
    Enable charities and individuals to advertise their available and required resources (human power, building material, etc) and its location.
  </li>
  <li>
    Provides a means for charities to discover information on the web relevant to their projects and location.
  </li>
</ol>

At the end it looked like this (test data, which probably doesn&#8217;t illustrate perfectly the usage):

[<img class="aligncenter" title="charity portal screenshot" src="{{ site.url }}/wp-content/uploads/2011/07/charity-portal-screenshot.png" alt="" width="1003" height="841" />][1]  
My team worked great together. We were able to quickly split the work so that everyone has something of interest to do. It was very exciting to see how we are making progress during our scrum stand ups:  
[<img class="aligncenter size-large wp-image-950" title="WP_000815" src="{{ site.url }}/wp-content/uploads/2011/07/WP_000815-1024x768.jpg" alt="" width="620" height="465" />][7]  
At the start of the event we decided that we are going to use Git for source control and GitHub for hosting. Apart from me only one other person had used Git before, but I was happy to help out each of the guys and keep them going during the event. As you can see from the below GitHub commits timeline we were quite effective in working in parallel:  
[<img class="aligncenter size-large wp-image-951" title="Weekend Timeline" src="{{ site.url }}/wp-content/uploads/2011/07/Weekend-Timeline-1024x105.png" alt="" width="620" height="63" />][8]  
I think 99.9% of us were running Windows and using Bash or Cmd was absolutely not an option for the non-Gitters who had mainly experience with TFS or SVN. Most new Gitters used Tortoise Git, but that was still confusing as it required understanding of how everything works. Only later I learned about SmartGit and in retrospect I think we should have used that instead.

As you&#8217;ll hear on the video initially we planned to use Orchard CMS, but after some research and poking around I was personally disappointed to find out how over-complex and over-engineered Orchard is. So we dumped it and went for plain old ASP.NET MVC 3 and non-POCO Entity Framework, which worked out quite well &#8211; after all it was just meant to be proof of concept.

All in all I had great fun during the weekend. I also met some great people, who I am looking forward to seeing and work with again in the future &#8211; probably during GiveCamp UK in October!

At the end of the weekend it turned out that most people voted for me as the most supportive and helpful developer during the weekend, so I won the grand prize &#8211; a Kinect. I wasn&#8217;t expecting this at all and very grateful to everyone and very happy that I was able to help people out. Thanks guys!  
[<img class="aligncenter size-medium wp-image-953" title="Kinect Prize" src="{{ site.url }}/wp-content/uploads/2011/07/Kinect-Prize-300x224.jpg" alt="" width="300" height="224" />][9]And here is an end of event photo of team dev4good:  
[<img class="aligncenter size-large wp-image-954" title="WP_000825" src="{{ site.url }}/wp-content/uploads/2011/07/WP_000825-1024x768.jpg" alt="" width="620" height="465" />][10]Great job guys!

Also big thanks to our sponsors:  
[<img class="aligncenter size-medium wp-image-955" title="sponsors" src="{{ site.url }}/wp-content/uploads/2011/07/sponsors-300x179.png" alt="" width="300" height="179" />][11]

### What&#8217;s next?

We are in the process of moving and merging the team repositories in a central one in GitHub. I am looking forward to get in touch with some charities to discuss and get feedback for our proof of concept Charity Portal and its eventual real-life practical application.

Next also is GiveCamp UK! I am also tempted to go to Startup Weekend Lonodon, but I&#8217;ll see about that.

 [1]: {{ site.url }}/wp-content/uploads/2011/07/charity-portal-screenshot.png
 [2]: http://dev4good.net
 [3]: {{ site.url }}/wp-content/uploads/2011/07/IMG_1305.jpg
 [4]: www.hopeandplay.org
 [5]: www.ministryofstories.org
 [6]: www.craftanddesigncouncil.org.uk
 [7]: {{ site.url }}/wp-content/uploads/2011/07/WP_000815.jpg
 [8]: {{ site.url }}/wp-content/uploads/2011/07/Weekend-Timeline.png
 [9]: {{ site.url }}/wp-content/uploads/2011/07/Kinect-Prize.jpeg
 [10]: {{ site.url }}/wp-content/uploads/2011/07/WP_000825.jpg
 [11]: {{ site.url }}/wp-content/uploads/2011/07/sponsors.png