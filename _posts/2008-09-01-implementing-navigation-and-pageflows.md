---
id: 23
title: Implementing Navigation and Pageflows with Seam
date: 2008-09-01T07:57:13+00:00
author: tshanky
guid: http://shanky.org/2008/09/01/implementing-navigation-and-pageflows/
permalink: /2008/09/01/implementing-navigation-and-pageflows/
categories:
  - Java and JVM
  - My Publications
tags:
  - JBoss
  - jPDL
  - Navigation
  - Seam
  - stateful
  - stateless
---
_**This article is published on <a title="Implementing Navigation and Pageflows" href="http://java.dzone.com/articles/implementing-navigation-and-pa" target="_blank">JavaLobby / Dzone</a>.**_ 

Seam is an interesting web framework that integrates the different pieces of the application stack and incorporates ideas from rules engines, BPM engines and SOA into regular application development. In this article, the focus is on Seam’s navigation and flow related features. Let’s see how they could play out in practice.
  
The article logically divides the topic into two parts, which map to the two navigation models, namely stateless and stateful. In Seam, stateless implementation uses JSF style pageflows while the stateful implementation uses jPDL pageflows.

### Navigation models

Navigation from page to page, or view to view, in a web application can either be agnostic to or be governed by the state of the application. In other words, it could be stateless or stateful. In a stateless model a set of rules map action or event outcomes to specific views or pages. On the contrary, in a stateful model the navigation rules define transitions from one state to the other, which in the process causes transition from one view or page to the other. You may ask why navigation models need to be stateful at all and you may also ask how the state information could be tied in with the navigation rules. Both the questions are pertinent and important so I will try and answer them right away.
  
In a process driven web application the flow from one page to the other is often governed by the application state and the decision tasks which use this state information. As an example, consider a loan processing application manifesting as a web application. In this application a user could be transitioned from the “welcome page” to the “loan information capture page” or “modify and confirm information page”. The choice of the page to transition to will depend on the applicant being an existing approved customer or not. An existing customer would be sent to the “modify and confirm information page” since most information pertinent to the loan processing is already available, while a new customer would be directed to the “loan information capture page” as basic information needs to be available before any further decisions can be made. Assume that such an application also gives the user a choice to upgrade its approval status. As an example the particular user may be approved for loan upto $500,000 but now needs to upgrade the loan approval amount to $1 million. In this example at the “modify and confirm information page” the user will choose to upgrade the loan amount and so would be diverted to “get additional information page”. You see what is happening here, the navigation is essentially mirroring the viable transitions which depend on the current state of the application. It’s a state transition scenario, that’s it! This flow is depicted in Figure titled: “The loan approval application process flow”.

<!--more-->

<img src="http://www.javaonseam.com/images/the_loan_approval_application_process_flow.png" alt="the_loan_approval_application_process_flow" width="650" height="500" align="middle" />

**Figure 1: The loan approval application process flow** 

The example clearly illustrates a need to define navigation as a set of rules that is constrained by the allowed transitions from the application state at every stage of the process. This is essentially what a stateful pageflow is all about and the example depicts a typical use case where it is applicable.
  
Next, let’s address the question of how one could implement stateful navigation rules. It is simple and there are many ways to do it. We need a mechanism to specify the process and a way to translate this process description into executable code. There are numerous process definition languages available today, that can successfully describe a process with simplicity. jPDL, BPEL and XPDL are examples of XML based process description languages. These define tags which describe tasks, states and decisions and allow processes to fork and join. Certainly, there are differences in terms of specific tag names and the complete vocabulary of these tags, among these process description languages. Workflow engines can execute the process defined in one or many of such languages. jPDL process descriptions can be successfully executed by jBPM, the JBoss workflow and business process management engine. Seam integrates with jPDL and utilizes jBPM to execute the described process. You do not need to be an expert in either jPDL or jBPM to use Seam and you can skip this entire facility if you don’t need stateful pageflows at all. However, Seam provides a clean and easy way to implement stateful pageflows and you will be benefited greatly by getting conversant with this option.

<!--more-->

<img class="mce_plugin_drupalbreak_pagebreak" title="<--pagebreak-->" src="http://java.dzone.com/sites/all/modules/tinymce/tinymce/jscripts/tiny_mce/themes/advanced/images/spacer.gif" alt="<--pagebreak->" width="100%" height="12" />

### JSF style pageflows

In the last section I was explaining the need for stateful pageflows and said that it’s beneficial to know more about it. However, stateless pageflows still dominates most use cases and so let’s start our conversation with that. After you get familiar with the stateless model we will discuss the stateful option as well.

Because, Seam was built initially to bridge the gap between EJB3 and JSF and integrate the JEE frameworks together, its user interface model uses JSF by default. In fact, it improves and extends the JSF features in many areas, including navigation definition. In the latest version, 2.0.2 RC2, the framework decouples from the JSF model. The process of decoupling was initiated a couple of revision back. If Seam is used without any JSF, what so ever, it would still be possible to define stateless navigation from one page to the other using the features we will describe soon.

A stateless navigation model, by definition, is unaware of the application state while transitioning from one page to the other. It relies on mapping pages to event or action outcomes. Think of it like the case-switch clause where the particular branch to be executed is determined by the case value it matches. Stateless decision making, under elementary situations can be compared to the bifurcation of a flow on the basis of a boolean property, which can be either true or false. The decision is based only on the case value and is not directly impacted by the application state. The action or event handler can generate the outcome on the basis of the application state but that is outside the scope of the navigation rules. What the navigation rule cares about is the outcome and not how the outcome is derived.

In JSF, stateless navigation rules are defined within faces-config.xml. The listing (number 1) below shows an example of JSF navigation definition. It’s possible to use this strategy with Seam, since Seam supports JSF.

<pre class="wp-code-highlight prettyprint">Listing 1: Example JSF navigation definition

&lt;navigation-rule&gt;

 &lt;from-view-id&gt;fromStartPage.jsp&lt;/from-view-id&gt;

 &lt;navigation-case&gt;

  &lt;from-outcome&gt;goAhead&lt;/from-outcome&gt;

  &lt;to-view-id&gt;toNextPage.jsp&lt;/to-view-id&gt;

 &lt;/navigation-case&gt;

 &lt;navigation-case&gt;

  &lt;from-outcome&gt;stopNow&lt;/from-outcome&gt;

  &lt;to-view-id&gt;byeNow.jsp&lt;/to-view-id&gt;

 &lt;/navigation-case&gt;

&lt;/navigation-rule&gt;</pre>

<!--more-->

JSF does not define a page action. This means that JSF triggers nothing special after it renders a page. No actions are activated or events are triggered. Seam defines page actions. Page actions and the related features can be used to define stateless pageflows and extend beyond the JSF pageflow model. Page actions are defined in a configuration file called pages.xml. So with Seam if one used JSF navigation and page flows at the same time, one would end up using two configuration files to manage the navigation rules. These would be faces-config.xml and pages.xml. However, it’s not necessary to use both the options simultaneously. Everything that can be done using the JSF navigation model can be done within pages.xml and so it may be prudent to consolidate everything right there.

Listing 2 to 4, shows snippets from the pages.xml file to illustrate Seam’s stateless navigation features. The example application in these listings is a blogging system that helps you create a new entry, edit an existing entry, preview it and publish it. It also lets you accept and respond to comments against these entries. Figure 2 depicts the application pages and features that relate to blog authoring pictorially.

<img src="http://www.javaonseam.com/images/flow_in_a_blog_authoring_system.png" alt="flow_in_a_blog_authoring_system" width="657" height="447" align="middle" />

**Figure 2: Flow in a blog authoring system**

Our first listing in this set, i.e. Listing 2, represents only the initial bifurcation between creating new document and editing existing document on the basis of the existence of the document. Listing 3 shows the flow along the new entry path while Listing 4 shows the flow along the path where we edit an existing document.

<pre class="wp-code-highlight prettyprint">Listing 2:  Seam navigation that either redirects to

new document creation or editing of an existing one

&lt;page view-id="welcome.xhtml"&gt;

 &lt;navigation from-action="#{documentToManipulate.exists}"&gt;

  &lt;rule if-outcome="true"&gt;

   &lt;redirect view-id="editDocument.xhtml"/&gt;

  &lt;/rule&gt;

  &lt;rule if-outcome="false"&gt;

   &lt;redirect view-id="createDocument.xhtml"/&gt;

  &lt;/rule&gt;

 &lt;/navigation&gt;

&lt;/page&gt;</pre>

<!--more-->

Did you notice that Listing 2 invoked a method called documentToManipulate(). This method returns a boolean type return value. The return value can thus be true or false.

In Listing 3, which shows navigation for a new document creation, you will notice a separate view for the action that saves a document. In real situations you may not desire a separate view to come up on saving a document to say that the document has been saved. It would be implicit.

<pre class="wp-code-highlight prettyprint">Listing 3: Navigation that defines the flow for creation of a new document

&lt;page view-id="createDocument.xhtml"&gt;

 &lt;navigation from-action="#{documentManager.preview}"&gt;

  &lt;rule if-outcome="#{documentManager.preview.assetTrue}"&gt;

   &lt;redirect view-id="previewDocument.xhtml"/&gt;

  &lt;/rule&gt;

 &lt;/navigation&gt;

 &lt;navigation from-action="#{documentManager.readyTosave}"&gt;

  &lt;rule if-outcome="yes"&gt;

   &lt;redirect view-id="saveDocument.xhtml"/&gt;

  &lt;/rule&gt;

&lt;/page&gt;

&lt;page view-id="previewDocument.xhtml"&gt;

 &lt;navigation from-action="#{documentManager.creationComplete}"&gt;

  &lt;rule if-outcome="success"&gt;

   &lt;redirect view-id="publishDocument.xhtml"/&gt;

  &lt;/rule&gt;

  &lt;rule if-outcome="failure"&gt;

   &lt;redirect view-id="editDocument.xhtml"/&gt;

 &lt;/navigation&gt;

&lt;/page&gt;</pre>

<!--more-->

Continuing with Listing 4, we see the navigation rule definition for flow around editing an existing blog entry. Only two actions, i.e. preview and save, are allowed for a blog being edited. The rule here only includes the flow to the preview page. You will notice that it is identical to the rule we define for preview in a new blog entry flow.

<pre class="wp-code-highlight prettyprint">Listing 4:  Navigation that defines the flow for editing an existing blog entry

&lt;page view-id="editDocument.xhtml"&gt;

 &lt;navigation from-action="#{documentManager.preview}"&gt;

  &lt;rule if-outcome="#{documentManager.preview.assetTrue}"&gt;

   &lt;redirect view-id="previewDocument.xhtml"/&gt;

  &lt;/rule&gt;

 &lt;/navigation&gt;

&lt;/page&gt;</pre>

The last of the listings in this set, i.e. Listing 5, is a hypothetical definition which tries to depict the extra features that Seam allows within the navigation rule definition in pages.xml. Some of the most important of these features relate to beginning and ending conversation during navigation and passing a request parameter during redirect. We have seen method evaluation outcomes as values for an action so far. Another interesting enhancement that Seam brings to navigation is to allow arbitrary EL expression outcomes to be values of an action.

<pre class="wp-code-highlight prettyprint">Listing 5: Additional Seam specific navigation features

&lt;page view-id="previewDocument.xhtml"&gt;

 &lt;navigation from-action="#{documentManager.creationComplete}"&gt;

  &lt;rule if-outcome="success"&gt;

   &lt;redirect view-id="publishDocument.xhtml"/&gt;

   &lt;end-conversation /&gt;

  &lt;/rule&gt;

  &lt;rule if-outcome="failure"&gt;

   &lt;redirect view-id="editDocument.xhtml"&gt;

    &lt;param name=”blogId” value=”#{documentManager.blogId}”/&gt;

   &lt;/redirect&gt;

 &lt;/navigation&gt;

&lt;/page&gt;</pre>

The stateless navigation as mentioned before is the most popular model in use. Seam’s features are adequate and flexible to implement these use cases effectively. However, with workflow and business process management application rising in number and highly interactive applications gaining in popularity, the stateful model is becoming equally important. While many options exist to manage stateful transitions, Seam is leading the way in integrating such constructs within web application frameworks.

<!--more-->

<img class="mce_plugin_drupalbreak_pagebreak" title="<--pagebreak-->" src="http://java.dzone.com/sites/all/modules/tinymce/tinymce/jscripts/tiny_mce/themes/advanced/images/spacer.gif" alt="<--pagebreak->" width="100%" height="12" />

### jPDL pageflows

jPDL is the process description language you will use to describe a process or a stateful pageflow within Seam. To get a flavor of the syntax and see it working in real life we will use an example. We will select an example where state transitions form the basis of all interactions. Our example is an online interactive game that involves two players where each player plays the alternate move. The outcome can be either one of the players winning or the game ending in a deadlock. Games like chess, tic-tac-toe, nim, dots and checkers fit into this type of interaction model. The source code for download implements the game of tic-tac-toe but it could equally well apply to any of the others on the list. Figure 3 shows the state transitions in such a game.

<img src="http://www.javaonseam.com/images/state_transition_in_a_two_player_game.png" alt="state_transition_in_a_two_player_game" width="657" height="358" align="middle" />

**Figure 3: State transitions in a two player game where either one of them wins or the game ends in a draw** 

The states are start, make a move, victory or impasse. Alternate moves continue till either one wins or a stalemate is reached. Such a flow can be written very effectively using jPDL. Listing 6 illustrates the state transitions for our example application using jPDL.

Listing 6 jPDL shows state transitions for a two player game where each player plays alternatively and either one wins or the game ends in a draw. This is a very elementary example as compared to what one may encounter in real business situations. However, it gives a flavor of process definition and state transitions in a stateful flow scenario.

<pre class="wp-code-highlight prettyprint">Listing 6: Process definition of the two player game

&lt;process-definition name="twoPlayerGame" initial="start&gt;

 &lt;state name="start"&gt;

  &lt;transition name="AStartsGame" to="APlays"&gt;

  &lt;transition name="BStartsGame" to="BPlays"&gt;

 &lt;/state&gt; &lt;state name="APlays"&gt;

  &lt;transition name="gameActive" to="BPlays"&gt;

  &lt;transition name="victory" to="AWins"&gt;

  &lt;transition name="impasse" to="draw"&gt;

 &lt;/state&gt;

&lt;state name="BPlays"&gt;

  &lt;transition name="gameActive" to="APlays"&gt;

  &lt;transition name="victory" to="BWins"&gt;

  &lt;transition name="impasse" to="draw"&gt;

 &lt;/state&gt;

&lt;/process-definition&gt;</pre>

The state transitions of this type could be implemented using Java within the action or event handler code. However, doing that would be neither easier nor as flexible. jPDL state transition changes can be made easily at one place in an XML file that resembles the natural language syntax. Changes to the code that manages state transitions in far more complicated and often not as decoupled. Also because it involves changes to the code rather than to a process description, it has to be done by a programmer. A person who has never programmed can also make changes to the XML file, since they are easy to understand.

### <!--more-->

### Summary

This article was about showing the implementation strategies as they are leveraged in practice. Now that the basics are well understood its time to start picking up complex cases and determine which option to choose to define navigation. Making a decision between stateless and stateful models is not always clear and straightforward. Experience above all usually helps make this decision effectively.

Understanding navigation models means getting a sense of the dynamics of interaction and flow control under varied scenarios. Once the interaction strategies are well understood it’s easier to deal with the user interface elements themselves and see how the interaction impacts their behavior and presentation logic. In the case of rich interactions as with AJAX and RIA, the navigation models not only pervade across layers but include multiple languages and sometimes an additional client side framework with Seam.

Therefore getting hold of the navigation model early on is going to help you go a long way in building a robust and scalable web application.