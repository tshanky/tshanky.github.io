---
id: 25
title: Flex HTTPService, Browser Cache and IE
date: 2008-08-02T11:24:03+00:00
comments: true
author: tshanky
layout: post
guid: http://shanky.org/2008/08/02/flex-httpservice-browser-cache-and-ie/
permalink: /2008/08/02/flex-httpservice-browser-cache-and-ie/
categories:
  - 'Interfaces -- HTML5/JS/Ajax/iOS/Android/Flex/AIR/PDF'
  - Public Events / Conferences
tags:
  - Browser Cache
  - flex
  - HTTPService
  - IE
---
This morning I spoke at a Flex event in Hyderabad (India). <a href="http://www.saventech.com" target="_blank">Our (Saven Technologies)</a> team members at Hyderabad planned and organized this fantastic event. It was a public event. Over 200 enthusiatic people attended the event.

I sincerely appreciate the effort of the organizers and thank the attendees for making it a successful event.

One of the attendees asked me a question about problems with Flex HTTPService and the IE browser cache. I promised to provide a detailed solution to the problem, so here it is:

**Problem:** Repeated HTTPService calls when made from Flex (running within an instance of the IE browser) many a times ends up with no external HTTP call. It appears the data is served from cache.

**Reason:**  The Flash Player piggybacks on the browser to make the HTTP call. IE caches the response from the HTTP GET calls and on occurrence of the same URL returns the response from the cache.

**Solution:** The problem can be solved either at the server side or at the client side.

> **Server side solution:**  Set the HTTP headers of the response to avoid returning response from cache.
> 
> In HTML: (in the header)
> 
> <META HTTP-EQUIV=&#8221;Cache-Control&#8221; CONTENT=&#8221;no-cache&#8221;>
  
> <META HTTP-EQUIV=&#8221;expires&#8221; CONTENT=&#8221;0&#8243;>
> 
> In PHP: (in the script)
> 
> header(”Cache-Control: no-cache, must-revalidate”);
  
> header(”Expires: Mon, 26 Jul 1997 05:00:00 GMT”);
> 
> In JSP: (before writing to the output stream)
> 
> response.setHeader(&#8220;Cache-Control&#8221;,&#8221;no-cache&#8221;);
  
> response.setDateHeader (&#8220;Expires&#8221;, 0);
> 
> **Client side solution:** (1) Make HTTP POST call &#8212; only HTTP GET calls are served from cache or (2) Make sure the HTTP GET URL is different every time.
> 
> (1) Make HTTP POST call &#8212;
  
> set method=&#8221;POST&#8221; and handle the call appropriately
> 
> (2) Append a unique parameter to the HTTP GET call so that the URL is different every time. A unique time stamp is a good choice.
  
> The following sample code, may do the job:
> 
> var timeStampForNocache:Date = new Date() ;
  
> params.noCacheControlVar = timeStampForNocache.getTime().toString() ;
  
> I have named the parameter &#8220;noCacheControlVar&#8221;. You can name it anything else you please. The name does not matter. What matters is that the timestamp makes the HTTP GET URL unique.

That&#8217;s it! Hope it helps and IE does not trouble you when using HTTPService anymore.

>