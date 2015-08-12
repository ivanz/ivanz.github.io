---
title: Banshee Xine backend HowTo
author: Ivan Zlatev
layout: post
permalink: /2006/07/23/banshee-xine-backend-howto/
views:
  - 271
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1316978
categories:
  - Coding
---
Ever wanted to use libxine to play stuff in [Banshee][1]? I&#8217;ve coded up the [Xine Backend][2] for you and here is how to install and activate it. Be aware that Banshee version newer than 0.10.10 is required. If you want to compile from source and you are on SUSE Linux you will have to install the *banshee-devel* package.

Download [XineEngine.dll][3] in the directory reported by

    pkg-config --variable=systemplugindir banshee | sed -e 's/Banshee.Plugins/Banshee.MediaEngine/'

Activate the engine with:

    gconftool-2 --set /apps/Banshee/PlayerEngine "xine-engine" --type string

Done!

<big><strong>From Source<br /> </strong></big>

    wget http://files.ivanz.com/projects/banshee/banshee-engine-xine/banshee-engine-xine.tar.gz
    tar xfz banshee-engine-xine.tar.gz
    cd banshee-engine-xine
    ./autogen.sh
    make
    make install

**<big>Changing the default engine in Banshee</big>**

Setting the default media engine in Banshee can be done by using gconftool:

    gconftool-2 --get /apps/Banshee/PlayerEngine
    gconftool-2 --set /apps/Banshee/PlayerEngine "engine-name" --type string

 [1]: http://www.banshee-project.org
 [2]: http://ivanz.com/projects/banshee/
 [3]: http://files.ivanz.com/projects/banshee/banshee-engine-xine/XineEngine.dll