---
id: 177
title: Prefixing Fx
date: 2009-02-14T07:26:31+00:00
comments: true
author: tshanky
layout: post
guid: http://shanky.org/?p=177
permalink: /2009/02/14/prefixing-fx/
categories:
  - 'Interfaces -- HTML5/JS/Ajax/iOS/Android/Flex/AIR/PDF'
tags:
  - Adobe Flex
  - Flash
  - flex
  - fx
  - FxButton
  - Namespace
  - prefix
---
Adobe&#8217;s Flex team seems inclined to move away from clean design.

Flex framework&#8217;s current stable version is 3.x and the Flex team at Adobe is actively working on getting the version 4.x ready this year. At this time, the core SDK of Flex 4, codenamed Gumbo, is evolving through an open source process. From peeking into its initial version, it looks promising as there are serious attempts to create a clean separation of behavior and presentation in the components, apart from the tons of nifty enhancements throughout the framework. (I promise to write about some of the forthcoming features soon)

However, one design decision in the midst of all this goodness seems rather odd and alarming. The Flex team is proposing to prefix Gumbo component names with the letters &#8220;Fx&#8221;. Lets try and understand what this means.

In Flex 3.x and 2.x you have a button component, which you access within a flex application using **<mx:Button>**. Here the name of the component is **Button** and **mx** is the namespace within which this component resides. Now in 4.x you still have the Button component but it deviates majorly from the component by the same name in the 3.x or the 2.x version of the framework. Its possible developers may like to use both versions concurrently and that&#8217;s where there is a need for the two entities, i.e. buttons, (with the same name) to be identified distinctly.

Simply said, how do you make sure the two versions of Button work together without causing confusion in the program. The answer to this question is simple and time tested: Put them in different logical buckets. 

This solution of putting things with same names into logically diferent partitions seems to work well in many situations. This is how two classes by the same name are differentiated &#8212; they are put under different package structures. For example a.b.c.Foo and x.y.z.Foo can co-exist as far as they are referred with their fully qualified names. This is how XML elements with the same name but from different schemas are reconciled. This is how variables with the same name but within different scopes are resolved.

However, this simple solution seems to evade the Flex team&#8217;s considerations. They believe logical partitioning (which translates to namespaces in the case of Flex components acessed by their XML tags) can be confusing for the beginners.

 

<div style="width: 373px" class="wp-caption alignnone">
  <a target="_blank" href="http://web.media.mit.edu/~lieber/Teaching/Common-Sense-Course-02/Which-Way-Up.jpg"><img title="Common Sense Reasoning" src="http://web.media.mit.edu/~lieber/Teaching/Common-Sense-Course-02/Which-Way-Up.jpg" alt="Common Sense Reasoning" width="363" height="382" /></a>
  
  <p class="wp-caption-text">
    Common Sense Reasoning
  </p>
</div>

So they propose instead that we prefix the names of all components whoes names clash with existing ones with the letters &#8220;Fx&#8221;. In other words our <mx:Button> in Flex 3.x or 2.x, which by the way is affectionately now called &#8220;Halo&#8221;,  becomes <FxButton> in Flex 4, which is also referred to as &#8220;Gumbo&#8221;.

If you use namespaces instead then this same Button in Gumbo will be **<fx:Button>**.

If we follow Adobe&#8217;s suggestion we may land up with something like this &#8212;

  * Halo &#8212; mx:Button
  * Gumbo &#8212; FxButton
  * Mumbo (or whatever they choose to affectionately call the next version) &#8212; Fx2Button or FxFxButton or MumboFxButton
  * Jumbo (possibly the following evolutionary version) &#8212;  Fx3Button or FxFxFxButton or JumboFxButton

Would you not just call a button a Button and have the namespaces decide weather its from the Halo, Gumbo, Mumbo or Jumbo clan?

If you would like to help Adobe make a sensible decision in favor of namespaces (i.e. using fx:Button instead of FxButton for now) then please go ahead and <a title="Replace Gumbo Fx Class Prefixing with a fx: namespace" href="https://bugs.adobe.com/jira/browse/SDK-17854" target="_blank">vote for this bug on the Flex JIRA</a>.