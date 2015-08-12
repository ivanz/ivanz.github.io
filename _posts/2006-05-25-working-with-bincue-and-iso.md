---
title: Working with .bin/.cue and .iso
author: Ivan Zlatev
layout: post
permalink: /2006/05/25/working-with-bincue-and-iso/
views:
  - 311
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1314732
categories:
  - Coding
---
Required: [bchunk][1], [mkisofs][2], [cdrecord][3].  
Target: Make an .iso from .bin/.cue, edit it, burn it to a disc and then create an .iso from the disk.

Let&#8217;s say we are in /home/user, where something.bin and something.cue can be found. The first thing to do is to install bchunk and convert those to an .iso:

<pre lang="bash">wget http://he.fi/bchunk/bchunk-1.2.0.tar.gz
tar -zxvf bchunk-1.2.0.tar.gz
cd bchunk-1.2.0
make
sudo make install
cd ..
bchunk something.bin something.cue something.iso</pre>

We will create a directory to temporary mount the .iso (this is the same as what Daemon Tools and Nero Image Drive will do for you on Windows) and then create a directory where we will copy the files from the .iso.

<pre lang="bash">mkdir tmp
mkdir iso_edit
mount -o loop -t iso9660 something.iso tmp/
cp -r /media/tmp/* iso_edit/</pre>

We don&#8217;t need the .iso to be mounted, so we can do a little bit of cleanup:

<pre lang="bash">umount tmp/
rm -rf tmp
rm something.iso</pre>

Next let&#8217;s add a description file to the files for the iso:

<pre lang="bash">cd iso_edit/
echo "My brand new edited ISO :)" > DESCRITPION
cd ..</pre>

To make the iso, write the md5 hash to a file and delete the temporary files:

<pre lang="bash">mkisofs -o new.iso -RJ iso_edit/
md5sum -b new.iso > MD5SUMS
rm -rf iso_edit/</pre>

To burn the .iso (/dev/XXX is your cd-rw drive):

<pre lang="bash">cdrecord -dev=/dev/hdb new.iso</pre>

To make an iso from the cd:

<pre lang="bash">dd if=/dev/cdrom of=new_copy.iso</pre>

 [1]: http://he.fi/bchunk/
 [2]: http://www.andante.org/mkisofs.html
 [3]: http://cdrecord.berlios.de/old/private/cdrecord.html