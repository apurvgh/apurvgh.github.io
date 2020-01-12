---
layout: post-default
title: CRM Plug-in Development - What should you be aware of?
date: 2012-9-18 1:48:0 - 0600
PostDate: 2012-9-18
author: apurvghai
ag_categories: Dynamics CRM SDK
lang: C#
---
<div class="blogSite">
<p align="justify">Today I&rsquo;m kicking off the posts on plugins. I dedicate today&rsquo;s post to CRM developers who are planning for plugin development. I have collected and provided all the information that you would need to understand before you start architecting your plugin. <br /> <br /> <b>Before we get started, please check the prerequisites that you should be aware of:</b></p>
<ul>
<li>CRM SDK 2011</li>
<li>Microsoft Visual Studio 2010 or higher</li>
</ul>
<p align="justify">Plug-ins are normal classes which are built using Class library project. They implement the IPlugin interface. All cases exposed in CRM 2011 are .NET Framework 4 CLR-compliant. You will need to add Microsoft.Xrm.Sdk.dll and Microsoft.Crm.Sdk.Proxy.dll assembly references to your project in order to compile plug-in code. All these assemblies can be found in the SDKBin folder of the SDK download. Refer to <a href="http://msdn.microsoft.com/en-us/library/gg594416"> http://msdn.microsoft.com/en-us/library/gg594416</a> to see the sample code for plugin.</p>
<p align="justify"><b>Designing a plugin</b> When you start thinking about the plugin make sure you have gone through the Event Execution pipeline described here <a href="http://msdn.microsoft.com/en-us/library/gg327941"> http://msdn.microsoft.com/en-us/library/gg327941</a></p>
<p align="justify"><b>Handling Exceptions</b> Here&rsquo;s is the sample to catch your exceptions: <a href="http://msdn.microsoft.com/en-us/library/gg327884"> http://msdn.microsoft.com/en-us/library/gg327884</a>. This sample contains the code applicable for Console Application. You can choose to write this into a file on a server. This is a business logic to be decided on the fly.</p>
<p align="justify"><b>Developing CRM Online Plugins vs. CRM OnPremise Plugins</b> When you start working on CRM online you might want to take care of not using IOrganizationService directly like you would do in custom SDK applications. You will land-up in Security Exception, the reason being sandboxed environment. Microsoft on cloud restricts the environment with unauthorized usage.</p>
<p>The sandbox environment is limited to access Registry settings, event logs etc. This is due to partial trust on assemblies. You might want to go through the definition of Sandbox environment <a href="http://en.wikipedia.org/wiki/Sandbox_%28computer_security%29"> http://en.wikipedia.org/wiki/Sandbox_%28computer_security%29</a>.</p>
<p>For more details on CRM Plugin Trusts, please read <a href="http://msdn.microsoft.com/en-us/library/gg334752"> http://msdn.microsoft.com/en-us/library/gg334752</a></p>
<p>Coming back to CRM OnPremise, You can allow your sandbox environments to make only web requests. This ideally means when you explicitly write connection to access CRM Web requests it will allow that. For this you will need to make registry changes. <br /> Registry Key: <i>HKEY_LOCAL_MACHINESOFTWAREMicrosoftMSCRMSandboxWorkerOutboundUriPattern</i> <br /> Value:<br /> ^http[s]?://(?!((localhost[:/])|([.*])|([0-9]+[:/])|(0x[0-9a-f]+[:/])|(((([0-9]+)|(0x[0-9A-F]+)).){3}(([0-9]+)|(0x[0-9A-F]+))[:/]))).+ <br /> Extract from: <a href="http://msdn.microsoft.com/en-us/library/gg334752"> http://msdn.microsoft.com/en-us/library/gg334752</a> Here&rsquo;s the sample for Web Access in plugin: <a href="http://msdn.microsoft.com/en-us/library/gg509030"> http://msdn.microsoft.com/en-us/library/gg509030</a></p>
<p><b>Debugging the plugin</b> When debugging an OnPremise plugin, please make sure you&rsquo;ve read <a href="http://msdn.microsoft.com/en-us/library/gg328574.aspx"> http://msdn.microsoft.com/en-us/library/gg328574.aspx</a> to check the name of processes which would be required to attach at the time of debug.</p>
<p>When debugging CRM Online Plugin you can make use of Plugin Profiler which is a part of Plugin registration tool. Here are the steps http://msdn.microsoft.com/en-us/library/hh372952.aspx</p>
<p><b>Registering Plugin</b> Please check out a walkthrough available at MSDN: <a href="http://msdn.microsoft.com/en-us/library/gg309580"> http://msdn.microsoft.com/en-us/library/gg309580</a></p>
<p><b>Custom SDK application vs. Plug-ins</b> When you start writing SDK application the first step that you would start with is with writing IOrganizationService to execute the functions like Create, delete and execute etc..,. When you start writing a plugin the service is already made available to you within in the context of it. Here&rsquo;s is the implementation /or sample of it http://msdn.microsoft.com/en-us/library/gg309673.aspx#bkmk_access.</p>
<p><b>Other topics that might interest you</b></p>
<ul>
<li>Pre and Post Entity Images <a href="http://msdn.microsoft.com/en-us/library/gg309673.aspx#bkmk_preandpost"> http://msdn.microsoft.com/en-us/library/gg309673.aspx#bkmk_preandpost</a></li>
<li>Input and Output Parameters <a href="http://msdn.microsoft.com/en-us/library/gg309673.aspx#bkmk_inputandoutput"> http://msdn.microsoft.com/en-us/library/gg309673.aspx#bkmk_inputandoutput</a></li>
</ul>
<p>Cheers, <br /> Apurv</p>
</div>
