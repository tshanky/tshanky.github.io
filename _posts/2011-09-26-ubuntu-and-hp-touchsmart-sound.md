---
id: 707
title: Ubuntu and HP TouchSmart Sound
date: 2011-09-26T16:04:01+00:00
author: tshanky
layout: post
guid: http://shanky.org/?p=707
permalink: /2011/09/26/ubuntu-and-hp-touchsmart-sound/
dsq_thread_id:
  - 4654810521
categories:
  - Ubuntu
---
I upgraded my Ubuntu install on my HP TouchSmart machine to version 11.04 (Natty Narwhal). Ubuntu 11.04 Unity Desktop experience is so nice and smooth that I started using my HP TouchSmart actively again. It had been sitting gathering dust for the last many months!

The last version of Ubuntu on this machine was 10.04, which was upgraded to 11.04, via a 10.10 upgrade en route. During 10.04 days, I had trouble getting Ubuntu to work smoothly on this machine. The internal speakers did not work (only external speakers did), the wifi did not work, and the touch screen lost its touch qualities. After I upgraded to 11.04, I somehow believed many of these past woes would get corrected but that wasn&#8217;t the case. So I actively started making some effort to resolve these issues. Getting the internal speaker sound to work was the first of the things I did and surprisingly a few minutes is all I needed to solve the problem.

The fix on the TouchSmart is really a simple and 1 line addition to a configuration file. Open the terminal and type the following:

<pre class="wp-code-highlight prettyprint">sudo gedit /etc/modprobe.d/alsa-base.conf</pre>

This will open alsa-base.conf in gedit, the official text editor on the Gnome desktop. If you like vi instead of gedit then open the file as follows:

<pre class="wp-code-highlight prettyprint">sudo vi /etc/modprobe.d/alsa-base.conf</pre>

At the very end add the following 1 line to alsa-base.conf file:

<pre class="wp-code-highlight prettyprint">options snd-hda-intel model=touchsmart</pre>

Now, save the file and reload alsa using:

<pre class="wp-code-highlight prettyprint">sudo alsa force-reload</pre>

and the internal speakers are in business. That was quick and simple. Wasn&#8217;t it?

**A little peek into why this fix works and how this may apply to systems other than the TouchSmart**:

Find out the model of your sound card using:

<pre class="wp-code-highlight prettyprint">cat /proc/asound/card0/codec* | grep Codec</pre>

On my TouchSmart the output is as follows:

<pre class="wp-code-highlight prettyprint">Codec: Analog Devices AD1984A</pre>

ALSA (Advanced Linux Sound Architecture) provides audio and MIDI functionality to the Linux OS. Browse the ALSA documentation to see list of supported audio models for your card. The documentation is available in /usr/share/doc/alsa-base/driver/HD-Audio-Models.txt.gz, which is a compressed file. You can list the content of this file, without decompressing, as follows:

<pre class="wp-code-highlight prettyprint">gunzip -c /usr/share/doc/alsa-base/driver/HD-Audio-Models.txt.gz</pre>

It  may be a good idea to page through the file using the more command like so:

<pre class="wp-code-highlight prettyprint">gunzip -c /usr/share/doc/alsa-base/driver/HD-Audio-Models.txt.gz | more</pre>

On my machine, I see the following entries relevant to AD1984A :

<pre class="wp-code-highlight prettyprint">....

AD1884A / AD1883 / AD1984A / AD1984B
====================================
desktop    3-stack desktop (default)
laptop    laptop with HP jack sensing
mobile    mobile devices with HP jack sensing
thinkpad    Lenovo Thinkpad X300
touchsmart    HP Touchsmart

....</pre>

(First column is the model and second one is the description)

This explains why the value of **snd-hda-intel model** was set to **touchsmart**. This hopefully also gives you a clue to find your sound card model and its supported configuration values for that model if you have a problem getting sound to work on your own Ubuntu install.

For additional reference, consider reading <a title="HdaIntelSoundHowto" href="https://help.ubuntu.com/community/HdaIntelSoundHowto" target="_blank">https://help.ubuntu.com/community/HdaIntelSoundHowto</a>.