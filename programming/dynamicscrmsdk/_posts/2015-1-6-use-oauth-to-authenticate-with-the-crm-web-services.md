---
layout: post-default
title: Use OAuth to Authenticate with the CRM Web Services
date: 2015-1-6 11:52:0 - 0600
PostDate: 2015-1-6
author: apurvghai
category: dynamicscrmsdk
category_title: Dataverse SDK
ag_original_link: https://learn.microsoft.com/en-us/archive/blogs/apurvghai/use-oauth-to-authenticate-with-the-crm-web-services
lang: C#
---
<p>Hey guys,<br /><br /> Today I'm going to talk about an interesting sample on Windows store apps which will go connect with CRM using Oauth.</p>
<p>You'll need a working knowledge on the following </p>
<ol>
<li><strong>ADAL</strong> (Active Directory Authentication Library) - Available on Nuget</li>
<li>ADFS 3.0</li>
<li>Window Store Apps</li>
<li>A bit of Fiddler (Optional)</li>
</ol>
<p>I'm trying to walk you through the entire steps which includes the configuration of ADFS, then we'll go ahead and build the sample.</p>
<h3>Register your application with ADFS Environment for development / Token issue</h3>
<ol>
<li>Create a GUID using Guid Generator using Visual Studio</li>
<li>Make a note of it for registration</li>
<li>At this point, I've not provided Redirect Url</li>
</ol>
<h3>Adfs script:</h3>
<p><span class="sourceJs"> Add-AdfsClient -ClientId C8E3F34A-E1A6-455C-A6DB-A1B82E6A6BE0 -Name WindowsAppsStore -Description "OAuth for CRM connecting Clients from Windows Store" </span></p>
<h3>Windows Store Application demo</h3>
<p>Note: This app is using <a href="https://code.msdn.microsoft.com/windowsapps/Mobile-Development-Helper-3213e2e6" target="_blank">Mobile SDK</a> Please read the instructions to compile and generate the Assembly for app reference</p>
<ol>
<li>Create a new project using Visual Studio 2013</li>
<li>Choose, Project for Store (Blank should be okay)</li>
<li>Add Reference to "Microsoft.Crm.Sdk.Mobile"</li>
<li>Add CrmHelper.cs file form the Mobile SDK Download to reuse the code.</li>
<li>I have written and attached below</li>
</ol>
<h3>Few Global Variables</h3>
<p><img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/windowstoreapps/3036.Global%20Variables.PNG" alt="" border="0" /></p>
<h3>ADAL Based Sample:</h3>
<p><img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/windowstoreapps/6116.Sample%20Code.png" alt="" border="0" /></p>
<p><em>It's important that you run the code for the first time with all the correct parameters/variables set and obtain the redirect URL from below code:</em> <br /> <span class="sourceJs"> public static string redirectUri = WebAuthenticationBroker.GetCurrentApplicationCallbackUri().ToString(); </span></p>
<p>Once you've see the value, it will look something like this <br /> <span class="sourceJs"> ms-app://s-1-15-2-23142 </span> <br /> Now go back to ADFS and use the below command: <br /><br /> <span class="sourceJs"> Set-AdfsClient -ClientId C8E3F34A-E1A6-455C-A6DB-A1B82E6A6BE0 -Name WindowsAppsStore -Description "OAuth for CRM connecting Clients from Windows Store" -RedirectUri ms-app://s-1-15-2-23142 </span></p>
<p>Once you execute and deploy your application. You can see the token value in fiddler as below:</p>
<pre>        HTTP/1.1 200 OK
        Cache-Control: no-store
        Pragma: no-cache
        Content-Length: 1115
        Content-Type: application/json;charset=UTF-8
        Server: Microsoft-HTTPAPI/2.0
        client-request-id: 43224044-4a95-41cc-9716-fe9387529c49
        Date: Tue, 06 Jan 2015 18:52:44 GMT
        {"access_token":"token value encrypted&gt;":491520}
</pre>
<p>My special credits to <a href="https://social.msdn.microsoft.com/profile/kenichiro%20nakamura/">Kenichiro Nakamura</a> for inspiring and providing technical insights to use Mobile SDK for CRM.</p>
<p>Cheers,<br /> Apurv</p>
<p><a href="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Components.PostAttachments/00/10/58/45/86/MainPage.xaml.zip">MainPage.xaml.zip</a></p>
