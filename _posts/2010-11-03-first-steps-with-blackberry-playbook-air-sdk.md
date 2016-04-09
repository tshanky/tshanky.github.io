---
id: 524
title: First Steps with BlackBerry PlayBook AIR SDK
date: 2010-11-03T15:05:38+00:00
comments: true
author: tshanky
layout: post
guid: http://shanky.org/?p=524
permalink: /2010/11/03/first-steps-with-blackberry-playbook-air-sdk/
categories:
  - 'Interfaces -- HTML5/JS/Ajax/iOS/Android/Flex/AIR/PDF'
---
Last week at <a title="Adobe MAX 2010" href="http://max.adobe.com/" target="_blank">Adobe MAX 2010</a>, the <a title="BlackBerry PlayBook" href="http://us.blackberry.com/playbook-tablet/" target="_blank">BlackBerry Playbook</a> was introduced to the world of developers. Positioned as a powerful multi-tasking environment it promises to pose formidable competition in the tablet landscape. The PlayBook OS is built on <a title="QNX" href="http://www.qnx.com/" target="_blank">QNX</a>, the real-time time-tested OS and offers an <a title="Adobe AIR" href="http://www.adobe.com/products/air/" target="_blank">Adobe AIR</a> based SDK for developers to write their apps.

As soon as I was back from MAX, I was itching to write my first &#8220;Hello World&#8221; app using the BlackBerry PlayBook SDK. So here in this post is an account of that adventure.

To get started with the PlayBook SDK, first go to the <a title="BlackBerry Tablet OS Development Resources" href="http://us.blackberry.com/developers/tablet/devresources.jsp" target="_blank">BlackBerry Tablet OS development resources</a> and download the SDK and the simulator for your platform. You will need to get the appropriate versions of the SDK and the simulator for your platform. At the moment, Mac and Windows are supported. I tried my hand with both Mac and Windows. There are &#8220;Getting started&#8221; guides for <a title="Getting Started Guide For Mac Developers" href="http://docs.blackberry.com/en/developers/deliverables/21878/" target="_blank">Mac</a> and <a title="Getting Started Guide For Windows Developers" href="http://docs.blackberry.com/en/developers/deliverables/21877/" target="_blank">Windows</a> and that&#8217;s where my journey began.

To setup the development environment you need the following:

  * The BlackBerry Tablet OS Simulator ISO
  * VMWare Player (or VMWare Fusion on the Mac) to configure and run a virtual environment on the basis of the ISO
  * <a title="Adobe AIR 2.5 SDK" href="http://labs.adobe.com/technologies/air/" target="_blank">AIR 2.5 SDK</a>
  * <a title="Flash Builder" href="http://www.adobe.com/products/flashbuilder/" target="_blank">Flash Builder</a> 4.x (the support officially extends to Flash Builder 4.0.1 but you can make it work with FB Burrito)
  * BlackBerry AIR SDK

The PlayBook simulator leverages <a title="VMWare Player" href="http://www.vmware.com/products/player/" target="_blank">VMWare Player</a> to create a virtual environment for you to test your apps. When I began to follow the steps in the &#8220;Getting started&#8221; started guide for Mac and wanted to download and install the VMWare Player on a Mac as per the following guidelines: <a title="Configure a virtual machine for the BlackBerry Tablet Simulator" href="http://docs.blackberry.com/en/developers/deliverables/21878/Configure_the_BlackBerry_Tablet_Simulator_1360962_11.jsp" target="_blank">Configure a virtual machine for the BlackBerry Tablet Simulator</a>, I realized a VMWare Player didn&#8217;t exist for a Mac at all. So I downloaded a trail version of <a title="VMWare Fusion" href="http://www.vmware.com/products/fusion/" target="_blank">VMWare Fusion</a> instead to get things working. Once VMWare Fusion is installed one can follow along the instructions in the &#8220;Getting started&#8221; guide to setup and configure the simulator. Since that is documented and available <a title="Configure a virtual machine for the BlackBerry Tablet Simulator" href="http://docs.blackberry.com/en/developers/deliverables/21878/Configure_the_BlackBerry_Tablet_Simulator_1360962_11.jsp" target="_blank">online</a> it makes no sense to reproduce it here.

Next, I downloaded the BlackBerry AIR SDK. Flash Builder 4.0.1, AIR 2.5 SDK and Flash Builder Burrito were already installed so I did not need to re-install it. If you don&#8217;t have Flash Builder and AIR on your machine then please download and install them before you move forward. Installing the BlackBerry SDK was elementary as a wizard guided through the process. However, one little thing was very odd. The BlackBerry SDK refused to deploy to any Flash Builder other than one named: &#8220;Adobe Flash Builder 4&#8221;. I have Flash Builder 4 installed as a plug-in and have the standalone FB Burrito that does not have a folder name as stated. So I had to hack around the problem by creating a symbolic link named &#8220;Adobe Flash Builder 4&#8221; to my FB Burrito folder on the Mac. On Windows I just configured the SDK, which I will talk about in a bit.

With that done things were working on the Mac.

On the Windows box, I had a different story. Installing the VMWare Player was no problem. It comes for free and installing it requires clicking the installer. That&#8217;s it! However, things got tricky right after that. I downloaded the simulator installer but on clicking the installer was greeted with this lack of 64-bit platform support message:

<div id="attachment_532" style="width: 310px" class="wp-caption alignleft">
  <a href="http://shanky.org/wp-content/uploads/2010/11/blackberry_tabletos_simulator_installer_error.png"><img class="size-medium wp-image-532  " title="BlackBerry TabletOS Simulator Installer Error on Windows 7" src="http://shanky.org/wp-content/uploads/2010/11/blackberry_tabletos_simulator_installer_error-300x118.png" alt="BlackBerry TabletOS Simulator Installer Error on Windows 7" width="300" height="118" /></a>
  
  <p class="wp-caption-text">
    BlackBerry TabletOS Simulator Installer Error on Windows 7
  </p>
</div>

This I think was quite uninviting. Anyway, I worked around this problem. All I needed from this bundle was the TableOS Simulator ISO, so I extracted the installer using <a title="7-zip" href="http://www.7-zip.org/" target="_blank">7-zip</a> and traversed down the extracted folder to BlackBerryPlayBookSimulator-Installer-Win\InstallerData\Disk1\InstData, where I again extracted the zipped up file called &#8220;Resource1&#8221;. Once extracted, I could get the ISO at Resource1\$IA\_PROJECT\_DIR$\installerdata. From there on I could use the instructions in the &#8220;Getting started&#8221; guide for Windows to setup the simulator.

As expected, the BlackBerry SDK installer didn&#8217;t work on Win 7 either. It again threw up the now familiar message stating lack of Win64 support. Makes me wonder why companies don&#8217;t have installers support the 64-bit platform. This is a developer&#8217;s tool and not an end user product. Many developers are already using the 64-bit platform and all should and will use it in a couple of years time.

I used the same trick as before and extracted the SDK installer using 7-zip. This time I traversed down the extracted installer to BlackBerryTabletSDK-Air-Installer-0.9.0-Win\InstallerData\Disk1\InstData. There I extracted the &#8220;Resource1&#8221; zip file and traversed further down to Resource1\$IA\_PROJECT\_DIR$\installerdata. In this folder there are two JAR (Java Archive) files that hold the contents of the SDK. The names of these 2 jar files are as follows:

  * blackberry-tablet-sdk-0.9.0\_zg\_ia_sf.jar
  * qnxsdk\_zg\_ia_sf.jar

Next, I extracted these two jar files using 7-zip again. After this I created a folder in the &#8220;applications&#8221; folder, which resides at the root of the C: drive and named it &#8220;blackberry-tablet-sdk-0.9.0&#8221;, essentially all except the &#8220;zg\_ia\_sf.jar&#8221; part of the BlackBerry Tablet jar file name. You can choose any other name, say just blackberry-tablet-sdk or any other. Next, I merged the contents of the two extracted jar files and the Adobe AIR 2.5 SDK (which I assume you would have downloaded and setup by now). That was it! It got my BlackBerry Tablet SDK up and running.

Once the SDK and simulator is setup, you can open up Flash Builder and first configure the BlackBerry Tablet AIR SDK by adding it to the list of &#8220;Installed Flex SDKs&#8221; as shown in the Figure below:

<div id="attachment_539" style="width: 492px" class="wp-caption alignleft">
  <a href="http://shanky.org/wp-content/uploads/2010/11/installed_flex_sdks.png"><img class="size-full wp-image-539 " title="Installed Flex SDKs" src="http://shanky.org/wp-content/uploads/2010/11/installed_flex_sdks.png" alt="Installed Flex SDKs" width="482" height="436" srcset="http://shanky.org/wp-content/uploads/2010/11/installed_flex_sdks-300x271.png 300w, http://shanky.org/wp-content/uploads/2010/11/installed_flex_sdks.png 803w" sizes="(max-width: 482px) 100vw, 482px" /></a>
  
  <p class="wp-caption-text">
    Installed Flex SDKs
  </p>
</div>

Once the setup was complete, I could finally get to writing the &#8220;Hello World&#8221; app. I wanted the &#8220;Hello World&#8221; app to be a bit more exciting than printing &#8220;Hello World&#8221; out to the screen so used the <a title="Google Maps API for Flash" href="http://code.google.com/apis/maps/documentation/flash/" target="_blank">Google Maps Flash API</a> to draw out a map of the area where the Adobe MAX venue, i.e. the LA Convention Center, was.

To create this app, I created a Flex project in Flash Builder and selected it to be an Adobe AIR application. Then I explicitly chose the BlackBerry Tablet AIR SDK as the SDK and finally I chose an ActionScript file as the main file. (As far as I understood, the SDK doesn&#8217;t support MXML directly as of now.)

Once the application was written, I compiled the application as usual and then packaged and installed the application to the simulator. I used the command line to package and install the app. You can read about the command line options <a title="Package and deploy your application from the command line" href="http://docs.blackberry.com/en/developers/deliverables/21877/Deploy_your_application_from_the_command_line_1347141_11.jsp" target="_blank">online</a>. You will need to retrieve the simulator IP to get this to work. Don&#8217;t forget to read about <a title="Retrieving the Simulator IP" href="http://docs.blackberry.com/en/developers/deliverables/21877/Retrieving_the_IP_address_of_the_simulator_1347133_11.jsp" target="_blank">retrieving the IP of the simulator</a>.

The command line packager and installer has a format like so:

_blackberry-airpackager -package output\_bar\_file\_name -installApp -launchApp project\_name-app.xml project\_name.swf any\_other\_project\_files -device IP_address_

I will not talk about the app itself at the moment but may cover that in a subsequent post, especially once I integrate with a few BlackBerry TabletOS specific features and gestures.

For now I just include a snapshot of the initial &#8220;Hello World&#8221; version.

<div id="attachment_544" style="width: 501px" class="wp-caption alignleft">
  <a href="http://shanky.org/wp-content/uploads/2010/11/google_maps_playbook_app_1.png"><img class="size-large wp-image-544  " title="Hello BlackBerry PlayBook World!" src="http://shanky.org/wp-content/uploads/2010/11/google_maps_playbook_app_1-1024x565.png" alt="Hello BlackBerry PlayBook World!" width="491" height="271" srcset="http://shanky.org/wp-content/uploads/2010/11/google_maps_playbook_app_1-300x165.png 300w, http://shanky.org/wp-content/uploads/2010/11/google_maps_playbook_app_1-1024x565.png 1024w, http://shanky.org/wp-content/uploads/2010/11/google_maps_playbook_app_1.png 1313w" sizes="(max-width: 491px) 100vw, 491px" /></a>
  
  <p class="wp-caption-text">
    Hello BlackBerry PlayBook World!
  </p>
</div>