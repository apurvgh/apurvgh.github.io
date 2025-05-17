---
layout: post-default
title: Creating CRM Early bound entity in CRM 2011 Plugin
date: 2013-4-3 16:42:0 - 0600
PostDate: 2013-4-3
author: apurvghai
category: dynamicscrmsdk
category_title: Dataverse SDK
ag_original_link: https://learn.microsoft.com/en-us/archive/blogs/apurvghai/creating-crm-early-bound-entity-in-crm-2011-plugin
lang: C#
---
<p>Hello Guys,</p>
<p>I&rsquo;m back with exciting new topic which could be helpful when you want to understand on how to use early bound classes in CRM Plugin.</p>
<p>Let&rsquo;s take a look at the hand-on pre-requisites</p>
<ul>
<li>Visual Studio 2010 or higher</li>
<li>.NET Framework 4.0</li>
<li>LINQ</li>
<li>CRM SDK - Consuming Services</li>
<li>CRM SDK - Plugin Development</li>
</ul>
<p>I wouldn&rsquo;t touch a lot of CRM SDK basics here. I&rsquo;d like to start with code generation tool directly commonly known as <strong>CrmSvcUtil</strong>. The below walkthrough will use a sample to update a contact on Post operation.</p>
<p>Let&rsquo;s go with step-by-step approach</p>
<ul>
<li>Create a new Class Library project in Visual Studio</li>
<li>Reference the below CRM SDK assemblies</li>
</ul>
<p style="padding-left:30px;"><img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/WebServiceForCrm/8524.crmassembly.png" alt="" border="0" /></p>
<ul>
<li>Add the below code</li>
</ul>
<div class="sourceCode"><span class="functionName">namespace</span> Apurv.CRMSdk.Samples <br /> {<br /> &nbsp;&nbsp;&nbsp;&nbsp; <span class="functionName">public class</span> <span class="greenCode"> UpdatePlugin </span>: <span class="greenCode">IPlugin</span><br /> &nbsp;&nbsp;&nbsp;&nbsp; {<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="functionName">public void </span>Execute(<span class="className">IServiceProvider</span> serviceProvider)<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // create organization service <br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span class="functionName">var</span> context = (<span class="className">IPluginExecutionContext</span>)serviceProvider.GetService(<span class="functionName">typeof</span>(<span class="className">IPluginExecutionContext</span>));<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span class="functionName">var</span> factory = (<span class="className">IOrganizationServiceFactory</span>)serviceProvider.GetService(<span class="functionName">typeof</span>(<span class="className">IOrganizationServiceFactory</span>));<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span class="functionName">var</span> Service = factory.CreateOrganizationService(context.UserId);<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span class="functionName">using</span> (<span class="functionName">var</span> crm = new <span class="className">XrmServiceContext</span>(Service))<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="greenCode">//Use this variable to fetch the current target </span> <br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="functionName">var</span> target = (<span class="className">Entity</span>)context.InputParameters["Target"];<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="greenCode">//Use this variable convert the target in current entity object</span><br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="functionName">var</span> entity = target.<span class="className">ToEntity</span>(); &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="greenCode">//Retrieving the Id value from the entity object.</span><br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="className">Guid id</span> = entity.Id.Value; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="greenCode">//Use the XRM contact object to pass in the Early-bound context. </span> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="className">contact</span> contactEntity = new <span class="className">contact</span> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{ contactEntity = id, contactEntity.lastname = "<span class="stringData">Last Name value</span>" };<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="greenCode">//In case you want check if the entity is attached or not. The below code can be used. </span><br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="functionName">if</span> (!crm.IsAttached(contactEntity)) <br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;crm.Attach(contactEntity); crm.UpdateObject(contactEntity);<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;crm.SaveChanges(); <br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; } <br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;} <br /> }</div>
<ul>
<li>Compile your code and you're assembly is ready for deployment.</li>
</ul>
<p>Here is this example we&rsquo;re getting the contact Id Guid from the Input Parameters which will help us to update the record in question. Once we save this record, the plugin will update the lastname to &ldquo;developer specified value&rdquo;. This example could be ideal scenario to create some auto-generated number via etc..,</p>
<p>Hope this was helpful.</p>
<p><strong>Cheers,<br /> Apurv </strong></p>
