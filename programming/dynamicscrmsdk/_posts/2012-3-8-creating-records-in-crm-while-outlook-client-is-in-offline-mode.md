---
layout: post-default
title: Creating records in CRM while Outlook client is in offline mode.
date: 2012-3-8 22:33:0 - 0600
PostDate: 2012-3-8
author: apurvghai
ag_categories: Dynamics CRM SDK
lang: C#
---
<div class="blogsite">
<p>Hello All, I&rsquo;m back with exciting new sample code to develop your own SDK application with offline capability. The application uses <strong>Cassini</strong> server URL. This article provides you step by step information how and what to write. I have also mentioned some environment step that you can keep handy.</p>
<h3>Requirements</h3>
<ol>
<li>CRM for Outlook (Offline Capability)</li>
<li>Visual Studio 2010</li>
<li>.NET Framework 4.0</li>
<li>CRM 2011 SDK</li>
</ol>
<h3>Walkthrough Steps:</h3>
<ol>
<li>Create a Windows application using Visual Studio</li>
<li>Add appropriate references and declare the following namespace</li>
</ol>
<p class="sourceCode"><span class="functionName">using</span> System.ServiceModel;<br /> <span class="functionName">using</span> System.ServiceModel.Description;<br /> <span class="functionName">using</span> Microsoft.Xrm.Sdk;<br /> <span class="functionName">using</span> Microsoft.Xrm.Client;<br /> <span class="functionName">using</span> Microsoft.Xrm.Sdk.Client;<br /> <span class="functionName">using</span> Microsoft.Xrm.Client.Services;<br /> <span class="functionName">using</span> Microsoft.Xrm.Client.Configuration;</p>
<ol>
<li>Add the following source code</li>
</ol>
<p class="sourceCode">//<span class="greenCode">Organization URL</span><br /> <span class="className">Uri</span> OrganizationUri = <span class="functionName">new</span> <span class="className">Uri</span>(<span class="functionName">String</span>.Format(<span class="stringData">"http://{0}/XRMServices/2011/Organization.svc"</span>,<span class="stringData">"localhost:2525"</span>)); <br /> //<span class="greenCode">Discovery URL</span><br /> <span class="className">Uri</span> HomeRealmUri = <span class="functionName">new</span> <span class="className">Uri</span>(<span class="functionName">String</span>.Format(<span class="stringData">"http://{0}/XRMServices/2011/Discovery.svc"</span>, <span class="stringData"> "localhost:2525"</span>));<br /> //<span class="greenCode">Domain Credentials</span><br /> <span class="className">ClientCredentials</span> localCredentials = <span class="functionName"> new</span> <span class="className">ClientCredentials</span>();<br /> //<span class="greenCode">Live Credentials - Used in CRM Online. I have just created instance, but I don't fill values.</span><br /> <span class="className">ClientCredentials</span> deviceCredentials = <span class="functionName"> new</span> <span class="className">ClientCredentials</span>();<br /> localCredentials.UserName.UserName = "<span class="stringData">XXXXXXXX</span>";<br /> localCredentials.UserName.Password = "<span class="stringData">XXXXXXXX</span>";<br /> <span class="functionName">using</span> (serviceProxy = <span class="functionName">new</span> <span class="className">OrganizationServiceProxy</span>(OrganizationUri, HomeRealmUri, localCredentials, deviceCredentials))<br /> {<br /> &nbsp;&nbsp;&nbsp;&nbsp;serviceProxy.ServiceConfiguration.CurrentServiceEndpoint.Behaviors.Add(<span class="functionName">new</span> <span class="className">ProxyTypesBehavior</span>());<br /> &nbsp;&nbsp;&nbsp;&nbsp; <span class="className">IOrganizationService</span> service = (<span class="className">IOrganizationService</span>)serviceProxy;<br /> &nbsp;&nbsp;&nbsp;&nbsp; <span class="className">Entity</span> account = new <span class="className">Entity</span>("account");<br /> &nbsp;&nbsp;&nbsp;&nbsp; accountId = serviceProxy.Create(account);<br /> &nbsp;&nbsp;&nbsp;&nbsp; <span class="className">MessageBox</span>.Show("Account with name " + textBox1.Text + " is created successfully.");<br /> }</p>
<p align="justify">This code is similar to what I have described in my first blog about creating web service <a href="http://blogs.msdn.com/b/apurvghai/archive/2011/08/26/create-your-custom-web-service-and-integrate-with-crm-2011-online.aspx">here</a> except I'm using cassini here instead of online URL.</p>
<p>You will also notice I have used <span class="stringData">"localhost:2525"</span> which is the default port for cassini server. You can always change the default port. I have complied couple of articles related to this topic.</p>
<ol>
<li>FAQ&rsquo;s on <strong>Cassini</strong> Server: <a href="http://support.microsoft.com/kb/893391"> http://support.microsoft.com/kb/893391</a></li>
<li>Sample : Use Outlook Methods: <a href="http://msdn.microsoft.com/en-us/library/gg309513.aspx"> http://msdn.microsoft.com/en-us/library/gg309513.aspx</a></li>
</ol></div>
