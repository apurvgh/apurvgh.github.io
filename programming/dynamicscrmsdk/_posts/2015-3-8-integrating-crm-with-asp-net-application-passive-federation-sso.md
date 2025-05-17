---
layout: post-default
title: Integrating CRM with ASP.NET Application (Passive federation - SSO)
date: 2015-3-8 14:39:0 - 0600
PostDate: 2015-3-8
author: apurvghai
category: dynamicscrmsdk
category_title: Dataverse SDK
ag_original_link: https://learn.microsoft.com/en-us/archive/blogs/apurvghai/integrating-crm-with-asp-net-application-passive-federation-sso
lang: C#
---
<p>Hey Guys, <br /><br /> I went back a little got this sample out from my source directory. Today I'm expanding a bit on existing sample for building an SSO based application to integrate with Dynamics CRM. I have collated few samples and concepts from below articles: <br /><br /></p>
<ul>
<li><a>"&gt;Walkthrough: Single Sign-on from a Custom Web Page</a></li>
<li><a href="http://blogs.msdn.com/b/crminthefield/archive/2013/10/30/crm-2011-and-asp-net-single-sign-on-use-wauth-for-integrated-web-apps.aspx" target="_blank"> CRM 2011 and ASP.NET Single Sign-on: Use WAUTH for Integrated Web Apps </a> (Thanks to Austin jones for this explanation)</li>
</ul>
<p>It's really important for you to understand on how WAUTH is used. The blog article above really helps you to understand this from the expert level point of view. To go more in depth about the SAML Based Authentication Context, you can see the below table at: <a href="https://msdn.microsoft.com/en-in/library/hh599318.aspx" target="_blank">Supported SAML Authentication Context Classes and Strengths</a></p>
<div>
<table border="1" cellspacing="0" cellpadding="1">
<tbody>
<tr>
<td><strong>Authentication Method</strong></td>
<td><strong>Authentication Context Class URI</strong></td>
</tr>
<tr>
<td>User Name and Password</td>
<td>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</td>
</tr>
<tr>
<td>Password Protected Transport</td>
<td>urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport</td>
</tr>
<tr>
<td>Transport Layer Security (TLS) Client</td>
<td>urn:oasis:names:tc:SAML:2.0:ac:classes:TLSClient</td>
</tr>
<tr>
<td>X.509 Certificate</td>
<td>urn:oasis:names:tc:SAML:2.0:ac:classes:X509</td>
</tr>
<tr>
<td>Integrated Windows Authentication</td>
<td>urn:federation:authentication:windows</td>
</tr>
<tr>
<td>Kerberos</td>
<td>urn:oasis:names:tc:SAML:2.0:ac:classes:Kerberos</td>
</tr>
</tbody>
</table>
<p>Now, I'll go ahead and walk you through the steps required to build your application:</p>
<ul>
<ul>
<li>Create a new Project "ASP.NET" Web Application in Visual Studio</li>
<li>You can choose to write the below sample to display the post login information as below:</li>
</ul>
</ul>
<img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/ssoifd/8400.Code%20Snippet%20-%20On%20Load.PNG" alt="" border="1" />
<ul>
<ul>
<li>Now go ahead and Publish your website in IIS / or Host your site.</li>
<li>While the hosting is done and mapped to your local path. I'd choose a custom header for my website to look like below</li>
</ul>
</ul>
<img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/ssoifd/7571.Select%20Custom%20Url%20in%20IIS.PNG" alt="" border="1" />
<ul>
<ul>
<li>I chose https://custom.domain.com:82/Welcome.aspx. Be sure to select your "Wild Card certificate". I'm using the same as I have for ADFS STS (With onebox setup)</li>
<li>Go ahead and download the <a href="http://www.microsoft.com/en-in/download/details.aspx?id=4451" target="_blank">WIF SDK</a></li>
<li>Go to Programs, and Select "<strong>Windows Identity Federation Utility Wizard" (FedUtil.exe)</strong> - <em>Please ensure to open this with "Run as Administrator" option</em>.</li>
<li>While you keep running this tool with Next option, you'll be providing below information</li>
&nbsp;&nbsp; - Custom Web Url: https://custom.domain.com:82/Welcome.aspx</ul>
</ul>
<ul>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; - STS Url: https://sts.domain.com/FederationMetadata/2007-06/FederationMetadata.xml</ul>
<ul>
<ul>
<li>I've added few screen shots below</li>
</ul>
</ul>
<img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/ssoifd/8203.STS%20Image.PNG" alt="" border="1" /><br /><img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/ssoifd/2867.Select%20Certificate.PNG" alt="" border="1" /><br />
<ul>
<ul>
<li>Now open Global.asax to use the code from <a href="http://blogs.msdn.com/b/crminthefield/archive/2013/10/30/crm-2011-and-asp-net-single-sign-on-use-wauth-for-integrated-web-apps.aspx">http://blogs.msdn.com/b/crminthefield/archive/2013/10/30/crm-2011-and-asp-net-single-sign-on-use-wauth-for-integrated-web-apps.aspx</a>. This will help you choose if you need IFD Redirected page or Claims Page. For demo I've just used this below:</li>
</ul>
</ul>
<img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/ssoifd/5672.Sign-in%20request.PNG" alt="" border="1" /><br />
<ul>
<li>These steps will be helpful for you to create your application.</li>
</ul>
<p>Happy CRM Integration :) <br /><br /> Cheers,<br /> Apurv</p>
</div>
