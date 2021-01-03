---
id: 353
title: Installing and Setting up BlazeDS in JBoss AS 5
date: 2009-08-21T16:26:40+00:00
author: tshanky
guid: http://shanky.org/?p=353
permalink: /2009/08/21/installing-and-setting-up-blazeds-in-jboss-as-5/
categories:
  - 'Interfaces -- HTML5/JS/Ajax/iOS/Android/Flex/AIR/PDF'
  - Java and JVM
tags:
  - BlazeDS
  - enterprise Java
  - flex
  - java
  - JBoss
---
In this article I illustrate how you could install and setup BlazeDS in a JBoss AS 5 application server instance. A number of new developers have difficulty installing and setting up BlazeDS in JBoss. I hope this article will provide answers to their questions and also help all those others who are seeking similar help.

First lets download and install JBoss AS 5. If you already have an instance installed then you can skip this step. If you have a prior version of JBoss (a 4.x version) installed then you could possibly avoid upgrading the installation, unless you need one of the newer features in version 5. You can read &#8220;<a title="What's new in JBoss AS 5?" href="http://www.jboss.org/file-access/default/members/jbossas/freezone/docs/Installation_And_Getting_Started_Guide/5/html/ch01.html" target="_blank">What&#8217;s new in JBoss AS 5</a>&#8221; to understand the new features in JBoss AS 5.

To download JBoss AS 5.1.0.GA (which is the latest stable release at the time of this writing), go to <http://jboss.org/jbossas/downloads/>. Once you have downloaded the distribution expand the archive file to a folder in your file system. JBoss AS is distributed in .zip and .tar.gz archive formats. If you are on windows, use the .zip distribution. Whereas, if you are on Mac OS X or Linux then get the .tar.gz version.

JBoss AS 5.1.0.GA needs JDK 6. If you don&#8217;t have JDK 6 on your machine please download and install it from <http://java.sun.com/javase/6/>.

Once you expand the JBoss distribution and you have the correct version of Java running on your machine, you are ready to start using JBoss. One of the thoughest things for a JBoss newbie is to understand the following:

  * the JBoss directory structure
  * the deployment model
  * the application server configuration
  * the startup and shutdown process

Therefore, in this write-up I will atttempt to explain the very basics of each of the topics in the list above and see how it applies to BlazeDS.

**The JBoss AS 5.x directory structure**

First look at the Figure 1 below.

<div id="attachment_364" style="width: 484px" class="wp-caption alignnone">
  <a rel="attachment wp-att-364" href="http://shanky.org/2009/08/21/installing-and-setting-up-blazeds-in-jboss-as-5/jboss_main_directory_structure-2/"><img class="size-full wp-image-364" title="jboss_main_directory_structure" src="http://shanky.org/wp-content/uploads/2009/08/jboss_main_directory_structure1.png" alt="Figure 1: JBoss main directory structure" width="474" height="226" srcset="http://shanky.org/wp-content/uploads/2009/08/jboss_main_directory_structure1-300x143.png 300w, http://shanky.org/wp-content/uploads/2009/08/jboss_main_directory_structure1.png 474w" sizes="(max-width: 474px) 100vw, 474px" /></a>
  
  <p class="wp-caption-text">
    Figure 1: JBoss main directory structure
  </p>
</div>

The &#8220;bin&#8221; and &#8220;server&#8221; folders are what you will need most. The &#8220;bin&#8221; folder has all the executables, including those to start and stop a server. The &#8220;server&#8221; is the core of the appplication server. This is where all the application server modules are and this where you deploy your application. The BlazeDS application, that you will download shortly will be deployed in a sub-folder of this folder.

The &#8220;lib&#8221;, &#8220;client&#8221; and &#8220;common&#8221; folders have all the jar files that JBoss server and client applications may need. The &#8220;docs&#8221; folder has the documentation.

Within the &#8220;server&#8221; folder you will find the following sub-folders:

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 819px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;">
  all
</div>

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 819px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;">
  default
</div>

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 819px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;">
  minimal
</div>

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 819px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;">
  standard
</div>

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 819px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;">
  web
</div>

  * all
  * default
  * minimal
  * standard
  * web

Each of these are built-in server profiles that you could use to deploy your application to. In most likelihood going with &#8220;default&#8221; will suffice. If none of these available server profiles are sufficient or suitable, you can also create your own custom server profile. For example I created a custom server profile called &#8220;sandbox&#8221; and started out by copying over all the contents of the &#8220;default&#8221; folder into it.

I will stick to the &#8220;default&#8221; server profile for the rest of this post and will not talk about custom server profiles for now. Within the &#8220;default&#8221; folder is a folder called &#8220;deploy&#8221;. The &#8220;deploy&#8221; folder is where you deploy an application within JBoss AS. This is a good time to download and deploy BlazeDS to show what deployment of a application in JBoss involves.

**Downloading and deploying BlazeDS**

Go to <http://opensource.adobe.com/wiki/display/blazeds/Downloads> and download the latest BlazeDS release build (which as of now is a 3.x version). BlazeDS can be downloaded either in source or binary formats. In addition, you have the option to download a BlazeDS turnkey distribution, which includes an Apache Tomcat instance and a set of sample applications that leverage BlazeDS, as a part of the distribution. I would recommend downloading the turnkey distribution to get hold of the sample applications. At the time of this writing the latest BlazeDS tunrkey release is &#8220;blazeds-turnkey-3.2.0.3978&#8221;.

Once downloaded, unzip the zipped up distribution. Once you expand the archive you will find the following &#8220;war&#8221; files in the root of the distribution:

  * blazeds.war
  * ds-console.war
  * samples.war

Next, copy over all the three &#8220;war&#8221; files to the **server/default/deploy** folder.  Now, you are ready to start the JBoss application server.

**Starting and stopping JBoss AS**

The JBoss AS &#8220;bin&#8221; folder has start and stop scripts. Scripts are available for multiple platforms, sepcially windows and the linux/unix/mac os x platforms. To start and stop a JBoss instance use &#8220;run.bat&#8221; and &#8220;shutdown.bat&#8221; on windows and &#8220;run.sh&#8221; and &#8220;shutdown.sh&#8221; on the linux/unix/mac os x platform. When using &#8220;run.bat&#8221; on Vista you may encouter problems with the script&#8217;s ability to locate findstr. Read more about this problem: <a title="Findstr Command Not Found" href="http://www.jboss.org/community/wiki/FindstrCommandNotFound" target="_blank">Findstr Command Not Found</a>.

Once the server starts, open up a browser instance and go to <a title="Sample applications" href="http://localhost:8080/samples" target="_blank">http://localhost:8080/samples</a> to access the sample applications that come with the BlazeDS turnkey distribution. In addition, you could point your browser to <a title="Administration console" href="http://localhost:8080/ds-console" target="_blank">http://localhost:8080/ds-console</a> and access the administration console that helps monitor the state of your BlazeDS instance.

At this stage if you peek into your JBoss server logs you are likely to see a trace that says its unable to connect to the sampledb. The error output on the command line is as shown in Figure 2.

<div id="attachment_391" style="width: 510px" class="wp-caption alignnone">
  <a rel="attachment wp-att-391" href="http://shanky.org/2009/08/21/installing-and-setting-up-blazeds-in-jboss-as-5/picture-2/"><img class="size-full wp-image-391" title="Picture 2" src="http://shanky.org/wp-content/uploads/2009/08/Picture-2.png" alt="Picture 2" width="500" height="119" srcset="http://shanky.org/wp-content/uploads/2009/08/Picture-2-300x71.png 300w, http://shanky.org/wp-content/uploads/2009/08/Picture-2.png 715w" sizes="(max-width: 500px) 100vw, 500px" /></a>
  
  <p class="wp-caption-text">
    Figure 2 : JBoss server log snippet
  </p>
</div>

To correct this problem, go to the place where you unzipped your blazeds turnkey distribution and run the &#8220;startdb&#8221; script within the &#8220;sampledb&#8221; folder.

BlazeDS is now installed and ready for use with a JBoss AS instance. More accurately you have taken the first few steps to start with more configuration and serious development. In later posts I will explain numerous configuration options around JMS, clustering, remoting and more. For now, I hope the write-up helps those who have had trouble with getting started with BlazeDS on JBoss.