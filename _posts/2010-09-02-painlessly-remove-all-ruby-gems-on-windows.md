---
id: 506
title: Painlessly Remove All Ruby Gems on Windows
date: 2010-09-02T01:12:14+00:00
comments: true
author: tshanky
layout: post
guid: http://shanky.org/?p=506
permalink: /2010/09/02/painlessly-remove-all-ruby-gems-on-windows/
dsq_thread_id:
  - 4654810475
categories:
  - Python/Perl/PHP/Ruby
tags:
  - gems
  - install
  - powershell
  - ruby
  - uninstall
---
If you want to uninstall all gems from your local Ruby installation on a Linux, Unix or MacOSX box then you can rely on the standard shell commands like &#8220;cut&#8221; and &#8220;xargs&#8221;, to make the process easy and effortless. The command itself is a one liner as follows:

<pre class="wp-code-highlight prettyprint">gem list | cut -d" " -f1 | xargs gem uninstall -aIx</pre>

Read this post titled &#8212; [Painlessly Remove All Ruby Gems](http://geekystuff.net/2009/1/14/remove-all-ruby-gems) &#8212; to learn the details. If you are on Windows though, the usual &#8220;cut&#8221; and &#8220;xargs&#8221; are not available. What are the alternatives then? One old school method may be to write a batch file or a script to do the job. However, that method is both a bit clumsy and verbose for a task that could be achieved through a one liner elsewhere. A smarter option then is to use the <a title="Windows PowerShell" href="http://technet.microsoft.com/en-us/scriptcenter/powershell.aspx" target="_blank">Windows PowerShell</a>. Lets see how.

First start a PowerShell instance. If you are on Windows 7, it would simply mean typing &#8220;PowerShell&#8221; (or even &#8220;powershell&#8221;) on your program search box and then selecting the &#8220;Windows PowerShell&#8221; program. Once a session is instantiated, type the usual

<pre class="wp-code-highlight prettyprint">gem list</pre>

command to list all the installed gems. On my machine the output looks like so:

<div id="_mcePaste">
  *** LOCAL GEMS ***
</div>

<div id="_mcePaste">
  abstract (1.0.0)
</div>

<div id="_mcePaste">
  actionmailer (3.0.0, 3.0.0.beta3, 2.3.5)
</div>

<div id="_mcePaste">
  actionpack (3.0.0, 3.0.0.beta3, 2.3.5)
</div>

<div id="_mcePaste">
  activemerchant (1.4.1)
</div>

<div id="_mcePaste">
  activemodel (3.0.0, 3.0.0.beta3)
</div>

<div id="_mcePaste">
  activerecord (3.0.0, 3.0.0.beta3, 2.3.5)
</div>

<div id="_mcePaste">
  activerecord-tableless (0.1.0)
</div>

<div id="_mcePaste">
  activeresource (3.0.0, 3.0.0.beta3, 2.3.5)
</div>

<div id="_mcePaste">
  activesupport (3.0.0, 3.0.0.beta3, 2.3.5)
</div>

<div id="_mcePaste">
  addressable (2.1.1)
</div>

<div id="_mcePaste">
  arel (1.0.1, 0.3.3)
</div>

<div id="_mcePaste">
  authlogic (2.1.3)
</div>

<div id="_mcePaste">
  builder (2.1.2)
</div>

<div id="_mcePaste">
  bundler (1.0.0, 0.9.25)
</div>

<div id="_mcePaste">
  buzzr (0.2)
</div>

<div id="_mcePaste">
  calendar_date_select (1.15)
</div>

<div id="_mcePaste">
  cgi_multipart_eof_fix (2.5.0)
</div>

<div id="_mcePaste">
  chronic (0.2.3)
</div>

<div id="_mcePaste">
  compass (0.8.17)
</div>

<div id="_mcePaste">
  couchrest (0.37)
</div>

<div id="_mcePaste">
  crack (0.1.7)
</div>

<div id="_mcePaste">
  data_objects (0.10.1)
</div>

<div id="_mcePaste">
  dbd-mysql (0.4.3)
</div>

<div id="_mcePaste">
  dbi (0.4.3)
</div>

<div id="_mcePaste">
  deprecated (2.0.1)
</div>

<div id="_mcePaste">
  dm-core (0.10.2)
</div>

<div id="_mcePaste">
  do_mysql (0.10.1 x86-mswin32-60)
</div>

<div>
  &#8230;.
</div>

To get a list of all installed gems, one unique entry per line, and containing nothing other than the names one can use the &#8220;cut&#8221; command on a Unix/Linux/MacOSX machine. With PowerShell though

<pre class="wp-code-highlight prettyprint">gem list | cut -d" " -f1</pre>

doesn&#8217;t work but

<pre class="wp-code-highlight prettyprint">gem list | %{$_.split(&#039; &#039;)[0]}</pre>

does. The $_ passes the current variable value to the &#8220;split&#8221; command, which uses a delimiter to split a string. In the example above a space is the delimiter. The parts generated out of the splitting are available as members of an array. Accessing the element at the 0th index of this array returns the first element.

Now that we have the names of all the installed gems, we need to iterate over this list and invoke gem uninstall with flags Iax for each of these. The I is for ignore dependency, a is for all matching gems and x is for no confirmation required. In other words running

<pre class="wp-code-highlight prettyprint">gem uninstall -Iax activerecord</pre>

should uninstall all gems matching the name &#8212; &#8220;activerecord&#8221; &#8212; without any required confirmation.  Using xargs it is easy to pass the current value to a command as one iterates over a list. However, running

<pre class="wp-code-highlight prettyprint">gem list | cut -d" " -f1 | xargs gem uninstall -aIx</pre>

doesn&#8217;t get the job done as xargs is unknown to PowerShell. Don&#8217;t be disappointed though for PowerShell has a replacement for xargs and its an elegant one. The position of the $_ makes all the difference. So the one liner that removes all ruby gems is:

<pre class="wp-code-highlight prettyprint">gem list | %{$_.split(&#039; &#039;)[0]} | %{gem uninstall -Iax $_ }</pre>

Isn&#8217;t that nice, simple and just a one liner!