---
layout: post-default
title: Create a new business process programmatically with multiple entities
date: 2015-2-13 16:38:0 - 0600
PostDate: 2015-2-13
author: apurvghai
category: dynamicscrmsdk
category_title: Dataverse SDK
ag_original_link: https://learn.microsoft.com/en-us/archive/blogs/apurvghai/create-a-new-business-process-programmatically-with-multiple-entities
lang: C#
---
<p>Hey Guys,</p>
<p>Today I'm going to talk about an exciting new feature of CRM 2013/2015 environment "Business Process Flows". You can read more about the features <a href="https://technet.microsoft.com/en-us/library/dn531164%28v=crm.6%29.aspx" target="_blank">here</a></p>
<p>While you can do this job from UI, but the idea is to automate the creation of next steps automatically. This article talks about the a plugin code using <em><span style="text-decoration:underline;"><strong>CRM SDK</strong></span></em> to achieve this. </p>
<p><br /> <strong> Some Prerequisites before you start:</strong></p>
<ol>
<li>Hands on CRM 2013 - Business process flow</li>
<li>Hands on CRM SDK 2013</li>
<li>Plugin Registration for CRM 2013</li>
<li>.NET Framework 4.0 / Visual Studio 2012 or higher</li>
</ol>
<p><strong>Consider the scenario below</strong>:</p>
<ul>
<li>Opportunity Entity</li>
<li>Custom Entity <strong>new_customflow</strong></li>
</ul>
<p>Please make sure you've enabled "<strong><em>Business Process Flows</em></strong>" in the entity customization for "<strong>new_customflow</strong>" entity, see below image:</p>
<p><img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/bflows/2110.Enabled%20Options.png" alt="" border="0" /></p>
<ul>
<li>Now go ahead and create a business flow with a custom entity</li>
</ul>
<p><strong> This will look like as shown below: </strong></p>
<p><img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/bflows/8255.Opportunity%20-%20Custom-%20Flows.png" alt="" border="0" /></p>
<p>You're all set with designing the business Flows via CRM customizations.</p>
<ul>
<li>Now when you create opportunity, it should automatically create a record for Custom Flows and link it to existing opportunity.</li>
</ul>
<p>You'll need to write the code as below</p>
<p><img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/bflows/8015.Source-code.png" alt="" border="0" /></p>
<ul>
<li>This code will be registered as Plugin</li>
</ul>
<p style="padding-left:30px;"><strong>For Plugins</strong> <em><strong>(Config)</strong></em>:<br /> a. Message: Create <br /> b. Entity: Opportunity<br /> c. Operation: Post<br /> d. Execution mode: Sync</p>
<p><span style="text-decoration:underline;"><strong>Note:</strong></span><em><strong> The workflow Id and Stage Id has to be correctly mentioned. In this sample, I've used hardcoded values which can be replaced by just doing a simple retrieve using SDK. Or you can use Early bound classes to use "WorkflowSet".</strong></em></p>
<p>You're now all set for automating your business process flows. As you hit "<em>next</em>" you can see the opportunity is now listed as connected flow. In the below screenshot, notice <strong>"108".</strong></p>
<p><span style="text-decoration:underline;"><strong>Stages preview</strong></span>:<br /><br /> <img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/bflows/2806.Stage%201%20-%20Co.png" alt="" border="0" /> <br /> <img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/bflows/0284.Stage%202%20-%20Co.png" alt="" border="0" /></p>
<p>I'd like to thank <a href="http://www.linkedin.com/in/kgiri/en"> Kaustubh Giri</a> who helped me with all the testing and innovating the business processes.<br /> <br /> <br /><br /> Happy customizing! :) <br /><br /> Thanks,<br /> Apurv</p>
