---
layout: post-default
title: Jscripts snippets for CRM 2011
date: 2011-11-21 19:9:0 - 0600
PostDate: 2011-11-21
author: apurvghai
category: dynamicscrmsdk
category_title: Dataverse SDK
ag_original_link: https://learn.microsoft.com/en-us/archive/blogs/apurvghai/jscripts-snippets-for-crm-2011
lang: C#
---
<div class="blogSite">
<p>I have tried to outline couple of examples that you could use in CRM. These examples are based on Xrm patterns which tells you how you can utilize the resources available in better way. These are newly introduced in CRM 2011. The whole idea is to put together a set of common functions as an library which can be used across the forms. You just need to register them under one common place called web resources and keep attaching them to forms where you need to use these. I will also demonstrate how you can achieve that. Let's get to the list and see what we have in store for today :)</p>
<ul>
<li>Server URL.</li>
</ul>
<div class="sourceJs">Xrm.Page.context.getServerUrl()</div>
<ul>
<li>Get the string value of a text field</li>
</ul>
<div class="sourceJs">function GetMainPhone() {<br /> &nbsp;&nbsp; var MainPhone = Xrm.Page.data.entity.attributes<br /> &nbsp;&nbsp; &nbsp;&nbsp;.get("telephone1").getValue();<br /> }</div>
<ul>
<li>Set the value of a string field</li>
</ul>
<div class="sourceJs">function SetMainPhone() {<br /> &nbsp;&nbsp;Xrm.Page.data.entity.attributes<br /> &nbsp;&nbsp; &nbsp;&nbsp;.get("telephone1").setValue();<br /> }</div>
<ul>
<li>Adding value to option set</li>
</ul>
<div class="sourceJs">var optionsetbox = Xrm.Page.getControl("customertypecode")<br /> optionsetbox.text= "Apurv"; <br /> optionsetbox.value= "10"; <br /> Xrm.Page.ui.controls.get("customertypecode").addOption(optionsetbox,12);</div>
<ul>
<li>Removing a value from option set on fly</li>
</ul>
<div class="sourceJs">Xrm.Page.getControl("customertypecode").removeOption(4);</div>
<p>I'm still working on more user friendly examples. Please do wait for more. Keep surfing and visiting.</p>
Cheers,<br /> Apurv</div>
