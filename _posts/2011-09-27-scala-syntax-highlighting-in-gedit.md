---
id: 728
title: Scala syntax highlighting in gedit
date: 2011-09-27T00:39:51+00:00
author: tshanky
guid: http://shanky.org/?p=728
permalink: /2011/09/27/scala-syntax-highlighting-in-gedit/
dsq_thread_id:
  - 4654810536
categories:
  - Java and JVM
  - Ubuntu
comments: true
---
> Update: A small typo, an unnecessary &#8220;<&#8221; tag before xmlns in scala-mime.xml has been corrected. Thanks @win for finding the error. See the comments below for additional references.

The default text editor on Ubuntu, or for that matter any Gnome powered desktop, is gedit. If you are a developer like me, who isn&#8217;t a huge fan of IDE(s), there is a good chance you use gedit for some of your development. Gedit supports syntax highlighting for a number of languages but if you were hacking some Scala code using the editor, you wouldn&#8217;t find any syntax highlighting support out-of-the-box. However, the Scala folks offer gedit syntax highlighting support via the scala-tool-support subproject. To get it working with your gedit installation, do the following:

  1. Download the **scala.lang** file from <a title="scala.lang for gedit" href="http://lampsvn.epfl.ch/trac/scala/browser/scala-tool-support/trunk/src/gedit/scala.lang" target="_blank">http://lampsvn.epfl.ch/trac/scala/browser/scala-tool-support/trunk/src/gedit/scala.lang</a>. You can checkout the source using svn or scrape the screen by simply copying the contents and pasting it into a file named **scala.lang**. On Ubuntu, using Ctrl-Shift and the mouse, helps accurately select and copy the content from the screen.
  2. Copy or move **scala.lang** file to **~/.gnome2/gtksourceview-1.0/language-specs/**
  3. Create a file named **scala-mime.xml** at **/usr/share/mime/packages/** using <pre class="wp-code-highlight prettyprint">sudo touch /usr/share/mime/packages/scala-mime.xml</pre>

  4. Add the following contents to **scala-mime.xml**: <pre class="wp-code-highlight prettyprint">&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;mime-info

 xmlns=&#039;http://www.freedesktop.org/standards/shared-mime-info&#039;&gt;

&lt;mime-type type="text/x-scala"&gt;

&lt;comment&gt;Scala programming language&lt;/comment&gt;

&lt;glob pattern="*.scala"/&gt;

&lt;/mime-type&gt;

&lt;/mime-info&gt;</pre>

  5. Run <pre class="wp-code-highlight prettyprint">sudo update-mime-database /usr/share/mime</pre>

  6. Start (or restart, if its running) gedit and you now have scala syntax highlighting in place.
