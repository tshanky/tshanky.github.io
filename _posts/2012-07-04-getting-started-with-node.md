---
id: 868
title: Getting Started With Node
date: 2012-07-04T23:28:49+00:00
author: tshanky
guid: http://shanky.org/?p=868
permalink: /2012/07/04/getting-started-with-node/
categories:
  - JavaScript
  - Real-time
---
<a title="node.js" href="http://nodejs.org/" target="_blank">Node.js</a>, the V8 (Chrome&#8217;s JavaScript runtime) based platform for building fast and scalable network applications,  is gaining substantial traction among developers and entering the application stack of many silicon valley companies. Like every new technology or piece of software there is enough FUD (Fear, Uncertainty, and Doubt) around Node, so I am going to write a few blog posts and help you learn Node by example.

In this post, I will simply help you get set-up so you can start playing with Node. The best and most reliable way to get Node installed on your machine is to build it from source. On Linux, Unix, and Mac OSX, you can start out by getting Node source code from its repository like so:

<pre class="brush: bash; title: ; notranslate" title="">git clone https://github.com/joyent/node.git</pre>

This assumes that you have a git client installed for your *nix flavor and you are familiar with the essential notions of git, like cloning a repository. If you are completely new to git, then you may want to quickly read and learn about git first. A good freely available resource on git is the book titled: Pro Git &#8212; <a href="http://git-scm.com/book/" title="Pro Git" target="_blank">http://git-scm.com/book/</a>.

Now that you have the Node source cloned on your machine, change to the source directory and inspect the available tags in the repository as follows:

<pre class="brush: bash; title: ; notranslate" title="">git tag -l</pre>

A whole lot of tags will be listed in response to this git command. Some of the latest ones are follows:

<pre class="brush: bash; title: ; notranslate" title="">...
v0.7.5
v0.7.6
v0.7.7
v0.7.8
v0.7.9
v0.8.0
v0.8.1
works
</pre>

The current master branch of the Node code is v0.9.x but unfortunatley that version seems to have problems working with NPM (Node Package Manager), a very important companion of Node. Therefore, you should checkout v0.8.1 before you build the source. To checkout the v0.8.1 tag, use the following command:

<pre class="brush: bash; title: ; notranslate" title="">git checkout v0.8.1</pre>

From here onwards, its the usual configure, make, and make install trio. Build Node as follows:

<pre class="brush: bash; title: ; notranslate" title="">./configure</pre>

<pre class="brush: bash; title: ; notranslate" title="">make</pre>

<pre class="brush: bash; title: ; notranslate" title="">sudo make install</pre>

That&#8217;s it! Node and NPM are both installed. 

To verify, open up a terminal and run

<pre class="brush: bash; title: ; notranslate" title="">node -v</pre>

If you see v0.8.1 in response then you are all set.

Additionally, verify that npm is installed by running

<pre class="brush: bash; title: ; notranslate" title="">npm -v</pre>

You should see 1.1.33.

Now that node is installed, you are ready to play with node. In my next post, we will get started with a simple example.