---
layout: post-default
title: Debugging CRM scripts using IE developer tool
date: 2012-3-8 23:5:0 - 0600
PostDate: 2012-3-8
author: apurvghai
ag_categories: Dynamics CRM SDK
lang: C#
---
<div class="blogsite">
<p>Hello people, I dedicate this post to our newest developers using jscript in CRM. I have added steps on how you can debug your scripts with events like OnLoad, OnSave and OnChange. The process of using IE developer doesn&rsquo;t change but I have customized it to CRM level.</p>
<h3>Requirements</h3>
<ol>
<li>Internet Explorer (7.0 or above)</li>
<li>CRM 2011 SDK</li>
<li>Jscript basics</li>
</ol>
<h3>Scenario</h3>
I want to display an alert on account form when I save the form.
<h3>Walkthrough Steps:</h3>
<ul>
<ul>
<li>Open the existing record</li>
<li>Press F12 on your keyboard, you will see below screen appearing</li>
</ul>
</ul>
<img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/IEdevelopercrm/5280.f12%20pressed.png" />
<ul>
<ul>
<li>Before even we start debugging, we need to identify the script which is already attached to this form. Click on the down arrow to look for your script/web resource. Below screen shot displays a list</li>
</ul>
</ul>
<img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/IEdevelopercrm/6648.list%20of%20scripts.png" />
<ul>
<ul>
<li>Finally I have searched my library which I want to debug, I click Start Debugging and you will see following screen. Select the line you want to debug</li>
</ul>
</ul>
<img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/IEdevelopercrm/8228.breakpoint%20setting.png" />
<ul>
<ul>
<li>Hit Save button to execute the script</li>
</ul>
</ul>
<img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/IEdevelopercrm/4300.press%20save.png" />
<ul>
<ul>
<li>You will see the debugger on your breakpoint line</li>
</ul>
</ul>
<img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/IEdevelopercrm/6116.debugger%20started.png" />
<p>For general article on using IE developer Tool, please read this <a href="http://msdn.microsoft.com/en-us/library/ie/dd565625%28v=vs.85%29.aspx"> http://msdn.microsoft.com/en-us/library/ie/dd565625%28v=vs.85%29.aspx</a></p>
<p>Cheers,<br /> Apurv</p>
</div>
