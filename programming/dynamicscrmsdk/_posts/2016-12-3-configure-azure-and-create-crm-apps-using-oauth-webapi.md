---
layout: post-default
title: Configure Azure and create dynamics 365 apps using OAuth, WebAPI (C#)
date: 2016-12-3 21:21:38 - 0600
PostDate: 2016-12-3
author: apurvghai
category: dynamicscrmsdk
category_title: Dataverse SDK
ag_original_link: https://learn.microsoft.com/en-us/archive/blogs/apurvghai/configure-azure-and-create-crm-apps-using-oauth-webapi
lang: C#
---
Hey Everyone

This blog covers the configuration steps that are required for app registration in Azure console and then consume WebAPI for creating and search existing records in CRM.

<span><strong>Prerequisites</strong> </span>:
<ol>
 	<li>Using Azure Management Console</li>
 	<li>Json Serialize using Newtonsoft</li>
 	<li>Basic understanding of HttpClient and WebAPI in general</li>
 	<li>ADAL (Azure Active Directory Authentication Library)</li>
</ol>
<strong>CONFIGURE YOUR APPLICATION USING AZURE MANAGEMENT CONSOLE:</strong>
<ol>
 	<li>Sign-up for Azure Trial/Office 365/Dynamics 365 (Formerly called as CRM Online) in a single tenant.</li>
 	<li>Launch your Azure Management Console and Navigate to Active Directory.</li>
 	<li><img width="589" height="116" src="http://apurvghai.files.wordpress.com/2016/12/main-ad-screen.png" class="attachment-266x266 aligncenter" alt="main-ad-screen" /></li>
 	<li>Register your application in Azure AD</li>
 	<li><a href="http://apurvghai.files.wordpress.com/2016/12/app-type-selection.png"><img src="https://msdnshared.blob.core.windows.net/media/2016/12/app-type-selection-300x141.png" alt="app-type-selection" width="300" height="141" class="size-medium wp-image-595 aligncenter" /></a><a href="http://apurvghai.files.wordpress.com/2016/12/tell-us-about-your-app.png"><img src="https://msdnshared.blob.core.windows.net/media/2016/12/tell-us-about-your-app-300x185.png" alt="tell-us-about-your-app" width="300" height="185" class="aligncenter wp-image-635 size-medium" /></a></li>
 	<li>Choose Native Client Application. You can read more about this in <a href="https://docs.microsoft.com/en-us/azure/app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication">Azure Documentation.</a></li>
 	<li>Once you've provided the information including Redirect Url. You should see the app dashboard as below</li>
 	<li><a href="http://apurvghai.files.wordpress.com/2016/12/azure-app-dash.png"><img src="https://msdnshared.blob.core.windows.net/media/2016/12/Azure-app-dash-300x167.png" alt="azure-app-dash" width="300" height="167" class="aligncenter size-medium wp-image-605" /></a></li>
 	<li>You've completed configuration! :)</li>
</ol>
&nbsp;

<strong>USING CONSOLE APPLICATION TO START YOUR VISUAL STUDIO PROJECT</strong>

<strong>//Retrieve Authentication Token from Azure</strong>

<a href="http://apurvghai.files.wordpress.com/2016/12/obtainauthtoken.png"><img src="http://apurvghai.files.wordpress.com/2016/12/obtainauthtoken.png" alt="obtainauthtoken" width="768" height="175" class="wp-image-615 aligncenter" /></a>

You will need to pass your ClientId and RedirectUrl from the azure management console. Step 7 shows the clientID which is hidden. That would 32 digit GUID. Here's the flow how OAUTH works. Read more on this <a href="https://blogs.msdn.microsoft.com/apurvghai/2016/12/11/troubleshooting-and-developing-oauth-enabled-applications-with-dynamics-365/">post</a>.

To learn more about recording records in CRM see this <a href="https://blogs.msdn.microsoft.com/apurvghai/2017/07/04/dynamics-365-webapi-crud-operations-c/">post</a>. If you'd like to understand the troubleshooting part with this sample, please read this <a href="https://blogs.msdn.microsoft.com/apurvghai/2016/12/11/troubleshooting-and-developing-oauth-enabled-applications-with-dynamics-365/">post</a>.

Hope you've had fun reading this. Happy WebAPI'ng :)

Thanks

Apurv

&nbsp;
