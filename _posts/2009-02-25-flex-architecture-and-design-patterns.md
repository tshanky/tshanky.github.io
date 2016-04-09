---
id: 205
title: Flex Architecture and Design Patterns
date: 2009-02-25T19:15:37+00:00
comments: true
author: tshanky
layout: post
guid: http://shanky.org/?p=205
permalink: /2009/02/25/flex-architecture-and-design-patterns/
dsq_thread_id:
  - 4654810395
categories:
  - 'Interfaces -- HTML5/JS/Ajax/iOS/Android/Flex/AIR/PDF'
tags:
  - Advanced Flex 3
  - Advanced Flex 4
  - Book
  - Design Patterns
  - Eventbrite
  - Flash
  - flex
  - Flex Architecture
  - 'Interfaces -- HTML5/JS/Ajax/iOS/Android/Flex/AIR/PDF'
  - Mentoring
  - new york
  - Training
---
A lot of my readers and clients have been asking for advice and help around getting Flex application architecture right. In some cases, these capable developers are struggling to morph their initial fancy toys into robust applications.

If you have seriously dabbled with Flex, you probably can empathise with them. However, if you haven&#8217;t delved into Flex at all or have minimally glanced at its surface, you are probably stunned in amazement and possibly ridiculing the indiscipline and lack of knowledge of these developers. Interestingly though, the shortcoming isn&#8217;t of the developers alone and the problems aren&#8217;t because the framework is flaky. Its just that you can code yourself into a corner despite your proficiency in MXML and AS3 and this problem is not new. The fact that: &#8220;fluency in a language and the core framework != fluency in building an application effectively using it&#8221; is well established across multiple languages. We have all seen similar problems surface with C++, Java, .Net, PHP, Python, Perl, Ruby, JavaScript and pretty much any other widely used language.

Over the years the community of software developers have questioned, theorized and debated over the root causes of failures emerging from bad application design and inappropriate architecture.  The viewpoints and thoughts are varied (An illustration of which is beyond the scope of this post. I may write about it in a separate post in future.) and there is no consensus on the right solution yet. However, there are some points of agreement and universal appreciation. One such topic of agreement, is the notion of leveraging design patterns. 

Design patterns have existed from the time the discipline of software development was a toddler back in the 70s, when it learnt to avoid its initial mistakes. Back then though, these patterns were not cataloged or adapted for specific areas of applicability. Now as the discipline is maturing into a teenager towards the end of the first decade of the 21st century, design patterns are entering the standard vocabulary of an average developer.

So, a Flex/AIR developer today can learn a lot of the theory about essential design patterns from the <a title="Design Patterns: Elements of Reusable Object-Oriented Software" href="http://www.amazon.com/Design-Patterns-Object-Oriented-Addison-Wesley-Professional/dp/0201633612/ref=pd_bbs_sr_1?ie=UTF8&s=books&qid=1235582910&sr=8-1" target="_blank">Gang of Four book</a> or browse through the <a title="Patterns of Enterprise Application Architecture (P of EAA)" href="http://martinfowler.com/eaaCatalog/" target="_blank">useful bunch of patterns applicable to enterprise application architecture</a>. In addition, he or she could pick up one of the two design patterns books that pertains to AS3, namely:

  * <a title="Advanced ActionScript 3 with Design Patterns" href="http://www.amazon.com/Advanced-ActionScript-3-Design-Patterns/dp/0321426568/" target="_blank">Advanced ActionScript 3 with Design Patterns</a>
  * <a title="ActionScript 3.0 Design Patterns: Object Oriented Programming Techniques" href="http://www.amazon.com/ActionScript-3-0-Design-Patterns-Programming/dp/0596528469/" target="_blank">ActionScript 3.0 Design Patterns: Object Oriented Programming Techniques</a>

Armed with all this knowledge, a Flex/AIR developer can hypothetically apply these gainfully to a real project. However, at this last link in the chain the story often breaks. Developers are left with tons of open questions around how to exactly use all their learning in the context of the core Flex framework features.

Its not a trivial effort to wire design patterns in conjunction with the existing framework classes. Using structural patterns with existing class heirarchies is not automatic and implementing behavioural patterns on top of the default flow is not intutive. In addition, you are left guessing on what could be implemented with AS3 alone and what could also rope MXML in.

At this stage, some developers just give up and some others seek refuge under any of the existing aggregations, especially if it seems to have official endorsement (read &#8220;Cairngorm&#8221;). Now, &#8220;giving up&#8221; can lead to code spaghetti and &#8220;seeking refuge&#8221; blindly can leave you in an obfuscated labyrinth, especially when you are deep into transforming your toy into a serious business application.

What then is a solution to the problem? How can one get Flex application architecture right?

To answer these questions to an extent, I wrote a chapter titled : &#8220;Leveraging Architectural and Design Patterns&#8221; in my book &#8212; <a title="Advanced Flex 3" href="http://www.amazon.com/AdvancED-Flex-3-Shashank-Tiwari/dp/1430210273/" target="_blank">Advanced Flex 3</a>. That chapter neither addressed all the issues not did it include details on implementing these patterns in Flex. It merely discussed the topic at a very high level. Even then, many found it immensely useful. Going by the positive feedback and the following questions from the readers, I could guess that the thirst to learn more about Flex design patterns remains unquenched.

Therefore, I am starting on 3 related yet distinct initiatives, which might help you all. These are:

  * Thorough hands-on Flex architecture mentoring sessions
  * Three chapters instead of one on architecture and design patterns in Advanced Flex 4 (the next version of <a title="Advanced Flex 3" href="http://www.amazon.com/AdvancED-Flex-3-Shashank-Tiwari/dp/1430210273/" target="_blank">Advanced Flex 3</a>)
  * A free book &#8212; &#8220;Flex Design Patterns&#8221; &#8212; on all aspects of architectural and design patterns in Flex. Chapters from which will be available for download right after they are written

In addition I am working actively on getting <a title="Fireclay" href="http://code.google.com/p/fireclay/" target="_blank">Fireclay</a> ready for prime-time. I hope [Fireclay](http://code.google.com/p/fireclay/ "Fireclay") will be a compelling and unique Flex framework when its version 1.0 is released.

If you would like to learn a lot by doing and want to gain substantial mastery in 3 days flat, then join me at the <a title="Flex Architecture BootCamp" href="http://flexdesignpatterns.eventbrite.com/" target="_blank">Flex Architecture BootCamp</a>, the first of which is coming to **New York** between **March 23 and March 25, 2009**. Find out the details about this event at the <a title="Flex Design Patterns" href="http://flexdesignpatterns.eventbrite.com/" target="_blank">Flex Design Patterns Eventbrite site</a>. 

At the Flex Architecture BootCamp, you will &#8212; 

  * Learn how to build enterprise grade Flex applications
  * Learn to leverage the common design patterns in Flex and ActionScript 3 applications
  * Understand what Cairngorm, PureMVC, Mate, Prana and Fireclay are all about
  * Learn to preempt problems involved in building complex enterprise grade Flex applications. Build applications reliable, scalable and performant from the beginning.

<img class="alignnone" src="http://shanky.org/images/flex_architecture_bootcamp_shankyorg.png" alt="" width="361" height="241" />

More information online at the <a title="Flex Design Patterns" href="http://flexdesignpatterns.eventbrite.com/" target="_blank">Flex Architecture BootCamp eventbrite</a> site. In a few days I will announce the schedule for this BootCamp at other cities, which include Chicago, Atlanta, Dallas and Seattle.

When you register for the bootcamp at New York, don&#8217;t forget to avail a $75 discount using **shanky_org** as the discount code.

In future posts, you will hear from me on when I may start writing Advanced Flex 4 (its definitely not happening till Flex 4 beta is out and that I think isn&#8217;t happening until May 2009).

Information on the free book &#8212; Flex Design Patterns &#8212; will be available soon.  I am currently trying to setup a repository and a methodology to manage the writing process. I am keen to use the docbook format and may use the GitHub to host all content and code. If you have any suggestions or recommendations on any alternative tools, then please chime in.

That it for now, but you know a lot&#8217;s coming, so stay tuned!