---
layout: post
title: "Running the Conrad Beagleboard Black (BBB)"
description: "This post has quick fixes for common problems with the Conrad Beaglebone Black (BBB)."
category: BBB, Beaglebone Black
tags: (bbb, beagleboard)
---
{% include JB/setup %}

# Preparing the Beagleboard Beaglebone Black (BBB)

This post has quick fixes for common problems I had with a Beagleboard Beaglebone Black (BBB) I bought in August 2013 at [Conrad] (http://www.conrad.de).

### 1. Deactivate G Data InternetSecurity 2014
If you are having problems getting the green "your are connected" image on the [local usb beagleboard page] (http://192.168.7.2), make sure you have deactivated the G Data InternetSecurity Web
<img src="/assets/2013-08-10-running-the-conrad-beagleboard-black-bbb/img/webschutz.jpg" />

### 2. Use "set date" in Beagleboard entry site
The date is always set to some 2000 at startup. This causes several problems with SSH, TAR, etc.
A quick fix is the use "set date" on the [local usb beagleboard page] (http://192.168.7.2)

<img src="/assets/2013-08-10-running-the-conrad-beagleboard-black-bbb/img/setdate.jpg" />

It is better to invest some minutes and follow the [real tutorial for setting the date on the BBB using NTP] (http://derekmolloy.ie/automatically-setting-the-beaglebone-black-time-using-ntp/)

### 3. Cloud9 is not running properly
In short: [do not use debug mode] (http://circuitco.com/support/index.php?title=BeagleBone_Black_FAQ)

### 4. Cannot connect to BBB with SSH
Neither PuTTY nor GateOne SSH client can connect to BBB.  
Look in the posts for the [Javascript for Cloud9 by Martin Schweizer] (https://groups.google.com/forum/#!msg/beagleboard/Ya2qE4repSY/73cyf1gOJB4J)

### 5. SD Card
Look in the posts for the [Post by William C Bonner] (https://groups.google.com/forum/#!msg/beagleboard/MKApMsH3Q7M/6eKKo2w_uCIJ)

### 6. Java on Beagle
Download the [ARM version] (http://www.oracle.com/technetwork/java/embedded/downloads/javase/index.html) (ARMv7 Linux - Headless - Server Compiler EABI, VFP, SoftFP ABI, Little Endian)

### 7. [Groovy Environment Manager (GVM)] (http://gvmtool.net/), [vert.x] (http://vertx.io/) and [Spring Boot] (https://github.com/SpringSource/spring-boot)
After installing the [Groovy Environment Manager (GVM)] (http://gvmtool.net/), you can use this to install vert.x and Spring Boot.  
For vert.x, there is a [detailed post] (http://touk.pl/blog/en/2013/02/23/vert-x-on-raspberry-pi/)

### 8. [SCons] (http://www.scons.org/)
For installing MongoDB you need a the build system [SCons] (http://www.scons.org/)
Follow the instructions [in the posts] (https://groups.google.com/forum/#!topic/beagleboard/UwtpcNdE7Kc)
<pre>
opkg update
opkg install python-distutils
opkg install python-compile
python setup.py install
</pre>

### 9. MongoDB 2.5.1
MongoDB is not quite ARM-friendly. You will have to use a quite outdated version of MongoDB, but it works.

<pre>
[WIN/BBB] git clone http://github.com/skrabban/mongo-nonx86 (I did this on Windows, as well as the replace step)
[BBB] opkg install boost
[BBB/WIN] find . -type f -exec sed -i 's/TIME_UTC/TIME_UTC_/' {} \; (or use UltraEdit with replace, then zip and copy with WinSCP to BBB)
[BBB] build as described in https://github.com/skrabban/mongo-nonx86/blob/master/docs/building.md
</pre>

#### 9a. Start MongoDB on startup
Add MongoDB to [startup scripts.] (http://unix.stackexchange.com/questions/45940/setting-up-boot-scripts-for-beaglebone-angstrom)  
*Whenever you have to change encoding, use UltraEdit or something similar on Windows, `dos2unix` did not work*