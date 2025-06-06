---
layout: post-default
title: Issues authenticating to CRM Online organizations with an error - An unsecured or incorrectly secured fault was received from the other party
date: 2012-9-10 0:3:0 - 0600
PostDate: 2012-9-10
author: apurvghai
category: dynamicscrmsdk
category_title: Dataverse SDK
ag_original_link: https://learn.microsoft.com/en-us/archive/blogs/apurvghai/issues-authenticating-to-crm-online-organizations-an-unsecured-or-incorrectly-secured-fault-was-received-from-the-other-party
lang: C#
---
<div class="blogSite">
<p align="justify">Hello Developers, <br /> This article is most interesting and life saver for who are in trouble with authenticating to CRM Online organizations. Microsoft recently announced about moving organization to integrated O365 environment. Looks awesome!! Does this mean you&rsquo;ll need to revisit your code?? Yes you will need to&nbsp;:) <br /> I&rsquo;ve framed couple of questions which may cross your mind.</p>
<p align="justify"><b>What has changed? </b> <br /> The CRM SDK has been revised with newer functions like &ldquo;Authenticate()&rdquo; to make our life easier. This returns security token response that is used in the service proxy constructor. We now use <a href="http://msdn.microsoft.com/en-us/library/hh547372.aspx" target="_blank">IServiceManagement Generic Interface</a> to pass through authentication. For more details, please check <a href="http://msdn.microsoft.com/en-us/library/hh670628.aspx"> http://msdn.microsoft.com/en-us/library/hh670628.aspx</a>.</p>
<p align="justify"><b>What error do I see when using older code against O365 based authentication? </b> <br /> The error is pretty generic and restricted to authentication failure in WCF layer. The typical error message is below<br /> <br /> <a href="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/2318.Unsecured%20Layer.png"> <img alt="" src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/2318.Unsecured%20Layer.png" border="0" /></a></p>
<p align="justify"><b>Do we have a sample?</b><br /> Yes we do have a sample code available <a href="http://msdn.microsoft.com/en-us/library/hh675404"> http://msdn.microsoft.com/en-us/library/hh675404</a> with helper class <a href="http://msdn.microsoft.com/en-us/library/gg309393"> http://msdn.microsoft.com/en-us/library/gg309393</a> which is revised to embed this part. You might want to look at <a href="http://blogs.msdn.com/b/apurvghai/archive/2012/07/29/authenticate-office-365-users-with-microsoft-dynamics-crm-online.aspx"> http://blogs.msdn.com/b/apurvghai/archive/2012/07/29/authenticate-office-365-users-with-microsoft-dynamics-crm-online.aspx</a>. I have simplified the approach for smooth usage.</p>
<p align="justify"><b>Is there any other thing I need to look at?</b><br /> Yes you need to be aware of discovery URL changes. We now use <a href="https://disco.crm.dynamics.com/XRMServices/2011/Organization.svc"> https://disco.crm.dynamics.com/XRMServices/2011/Organization.svc</a> Please refer this article <a href="http://msdn.microsoft.com/en-us/library/gg309401.aspx">http://msdn.microsoft.com/en-us/library/gg309401.aspx</a> to choose correct endpoints.</p>
<p>I also noticed a blog post available from Microsoft talking about ongoing issues with CRM Online organizations. Check out for more insights <a href="https://community.dynamics.com/product/crm/crmtechnical/b/crmonlineservice/default.aspx"> https://community.dynamics.com/product/crm/crmtechnical/b/crmonlineservice/default.aspx</a></p>
Hope this helps!! Let me know if you have something I can add here :) <br /> <br /> Cheers,<br /> Apurv</div>
