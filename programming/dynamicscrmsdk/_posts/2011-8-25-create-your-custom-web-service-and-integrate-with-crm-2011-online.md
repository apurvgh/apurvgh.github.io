---
layout: post-default
title: Create your custom web service and integrate with CRM 2011 Online
date: 2011-8-25 15:56:0 - 0600
PostDate: 2011-8-25
author: apurvghai
category: dynamicscrmsdk
category_title: Dataverse SDK
lang: C#
ag_original_link: https://learn.microsoft.com/en-us/archive/blogs/apurvghai/create-your-custom-web-service-and-integrate-with-crm-2011-online
---
<p class="blogsite">I hope this article can help you understand when you need to create a middleware between CRM system and a Legacy system/or custom application.The below example talks about the &ldquo;<strong>contact</strong>&rdquo; entity where the web service accepts the &ldquo;<strong><em>lastname</em></strong>&rdquo; attribute of it and inserts the same into CRM system. The end result is the guid which is retrieved and confirms the successful execution of the operation.</p>
<h4 class="blogsite">Before we get started, here&rsquo;s the list of requirements</h4>
<div class="blogsite"><ol>
<li>Microsoft Visual Studio 2010</li>
<li>MS .NET Framework 4.0</li>
<li>MS CRM SDK 2011</li>
</ol></div>
<h3>Walkthrough Steps: Web Service</h3>
<ul>
<li>Open Visual Studio 2010</li>
<li>Select your project, ASP.NET Empty Web Application</li>
<li>Select a suitable name of your project</li>
<li>On Successful Creation of Project</li>
<li>Go to Solution Explorer</li>
<li>Add New Item: Web Service</li>
</ul>
<p><br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/WebServiceForCrm/8637.ProjectNaming.png" /><br />&nbsp;</p>
<ul>
<li>Provide a suitable name</li>
<li>Click on Add to create</li>
<li>You will see the C# content in &ldquo;CustomWebService.asmx.cs&rdquo;</li>
<li>Add reference to CRM Assemblies</li>
</ul>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/WebServiceForCrm/8524.crmassembly.png" /></p>
<ul>
<li>Adding another .NET assemblies</li>
</ul>
<p>&nbsp;<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/WebServiceForCrm/3872.dotNetAssemblies.png" /></p>
<ul>
<li>Adding Live ID Code File available in CrmSdk (SDKsamplecodecshelpercode)</li>
<li>Declare the namespace</li>
</ul>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/WebServiceForCrm/7674.Namespaces.png" width="301" height="87" /></p>
<ul>
<li>Write the code for CRM</li>
</ul>
<p></p>
<div class="sourceCode">/// &lt;summary&gt;<br /> /// <span class="greenCode">This function accepts the lastname and creates a contact with that in CRM</span><br /> /// &lt;/summary&gt;<br /> /// &lt;param name="lname"&gt;&lt;/param&gt;<br /> /// &lt;returns&gt;&lt;/returns&gt;<br /> <span class="functionName">public</span> string <span class="className">CreateContact</span>(<span class="functionName">string</span> lname)<br /> {<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="functionName">string</span> message = <span class="functionName">string</span>.Empty;<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="functionName">try</span><br /> &nbsp;&nbsp;&nbsp;&nbsp; {<br /> &nbsp;&nbsp;&nbsp;&nbsp; <span class="className">OrganizationServiceProxy</span> serviceProxy;<br /> &nbsp;&nbsp;&nbsp;&nbsp; <span class="className">ClientCredentials</span> deviceCredentials = null;<br /> &nbsp;&nbsp;&nbsp;&nbsp; <span class="className">ClientCredentials</span> clientCredentials = null;<br /> &nbsp;&nbsp;&nbsp;&nbsp; <span class="className">Guid</span> contactId = Guid.Empty;<br /> &nbsp;&nbsp;&nbsp;&nbsp; <br />&nbsp;&nbsp;&nbsp;&nbsp; <span class="greenCode">//Organization URL</span><br /> &nbsp;&nbsp;&nbsp;&nbsp; <span class="className">Uri</span> OrganizationUri = new <span class="className">Uri</span>(<span class="functionName">String</span>.Format(<span class="stringData">"https://{0}.api.crm.dynamics.com/XRMServices/2011/Organization.svc","&lt;&lt;Organization&gt;&gt;"</span>));<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="greenCode">//Discovery URL</span><br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="className">Uri</span> HomeRealmUri = new Uri(String.Format(<span class="stringData">"https://dev.{0}/XRMServices/2011/Discovery.svc", "crm.dynamics.com"</span>));<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="greenCode">//Setting Client Credentials</span><br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;clientCredentials = <span class="functionName">this</span>.GetCredentials(<span class="stringData">"&lt;&lt;Live Id&gt;&gt;"</span>, <span class="stringData">"&lt;&lt;Live Password&gt;&gt;"</span>);<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="greenCode">//Get the Device Id and Password</span><br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;deviceCredentials = this.GetDeviceCredentials();<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="greenCode">//Using Organization Service Proxy to instantiate the CRM Web Service</span><br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="functionName">using</span> (serviceProxy = new <span class="className">OrganizationServiceProxy</span>(OrganizationUri, HomeRealmUri, clientCredentials, deviceCredentials))<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="greenCode">&nbsp;// This statement is required to enable early-bound type support.</span><br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serviceProxy.ServiceConfiguration.CurrentServiceEndpoint.Behaviors.Add(<span class="functionName">new</span> <span class="className">ProxyTypesBehavior</span>());<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="className">IOrganizationService</span> service = (IOrganizationService)serviceProxy;<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="greenCode">//Define Entity Object</span><br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span class="className">Entity</span> contact = new Entity("contact");<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="greenCode">&nbsp;//Specify the attributes</span><br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;contact.Attributes["lastname"] = lname;<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="greenCode">//Execute the service</span><br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;contactId = service.Create(contact);<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="greenCode">//Confirmation Message</span><br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; message = <span class="stringData">"Contact Created with LastName:- "</span> + lname + <span class="stringData">" Guid is"</span> + contactId.ToString();<br /> <br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="functionName">catch</span> (<span class="className">Exception</span> ex)<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; message = ex.Message;<br /> &nbsp;&nbsp;&nbsp;&nbsp; }<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="greenCode">//returns the message</span><br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="functionName">return</span> message;<br /> }</div>
<p></p>
<ul>
<li>Write the other functions used in the code above</li>
</ul>
<p></p>
<div class="sourceCode">
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span class="functionName">protected</span> <span class="functionName">virtual</span> <span class="className">ClientCredentials</span> GetCredentials(<span class="functionName">string</span> username, <span class="functionName">string</span> password)<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span class="className">ClientCredentials</span> credentials = new <span class="className">ClientCredentials</span>();<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; credentials.UserName.UserName = username;<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; credentials.UserName.Password = password;<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span class="functionName">return</span> credentials;<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span class="functionName">protected virtual</span> <span class="className">ClientCredentials</span> GetDeviceCredentials()<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span class="functionName">return</span> Microsoft.Crm.Services.Utility.<span class="className">DeviceIdManager</span>.LoadOrRegisterDevice();<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }</p>
</div>
<p></p>
<ul>
<li>Complie your code</li>
<li>You've successfully completed the walkthrough.</li>
</ul>
<p><strong>Useful links</strong></p>
<ol>
<li><a href="http://msdn.microsoft.com/en-us/library/gg328149.aspx" target="_blank">How to use retrieve method</a></li>
</ol>
