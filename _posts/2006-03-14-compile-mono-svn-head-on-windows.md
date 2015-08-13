---
title: Compile Mono SVN Head on Windows
author: Ivan Zlatev
layout: post
permalink: /2006/03/14/compile-mono-svn-head-on-windows/
views:
  - 1493
aktt_notify_twitter:
  - no
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1333245
categories:
  - Coding
tags:
  - HowTo
  - Mono
---
  1. Download and install a recent Mono from <a title="Mono's downloads section" href="http://mono-project.com/Downloads" target="_blank">http://mono-project.com/Downloads</a>. Install it in *C:\mono* (instead of C:\Program Files\Mono) in order to avoid problems with whitespace in path during build). This mono will be used to compile mono from source. It will also provide a set of other native libraries required for the build.
  2. Install [Cygwin][1] (I would recommend in *C:\cygwin* to same problems mentioned above) with the following packages in addition to the default ones:
    autoconf
    automake
    bison
    gcc-mingw
    gcc-mingw-g++
    libtool
    pkg-config
    subversion
    make
    

  3. Downgrade *make *because version 3.81 installed by Cygwin introduces a bug which causes the mono build to fail. Replace *C:\cygwin\bin\make.exe *with the one from here: [make 3.80][2]
  4. Prepare the build environment by setting the paths so that all the dependancy libs and scripts will be used from the official setup.
    export MONO_LOCATION=/cygdrive/c/mono
    export PATH=${PATH}:${MONO_LOCATION}/bin
    export ACLOCAL_FLAGS="-I ${MONO_LOCATION}/share/aclocal"
    export PKG_CONFIG_PATH=${PKG_CONFIG_PATH}:${MONO_LOCATION}/lib/pkgconfig
    mkdir -p /mono/svn
    mkdir /mono/build
    

  5. Checkout Mono&#8217;s source from svn and compile.
    cd /mono/svn
    svn co svn://anonsvn.mono-project.com/source/trunk/mono
    svn co svn://anonsvn.mono-project.com/source/trunk/mcs
    cd mono
    ./autogen.sh --prefix=/mono/build
    make
    make install

  6. Copy the freshly compiled files from *C:\cygwin\mono\build *to *C:\mono* and replace all existing. DONE!

 [1]: http://www.cygwin.com
 [2]: {{ site.url }}/wp-content/uploads/2006/03/make-380-1tar.bz2