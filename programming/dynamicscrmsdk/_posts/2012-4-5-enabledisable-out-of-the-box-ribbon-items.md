---
layout: post-default
title: Enable/Disable out of the box ribbon items
date: 2012-4-5 0:33:0 - 0600
PostDate: 2012-4-5
author: apurvghai
ag_categories: Dynamics CRM SDK
lang: C#
---
<div class="blogsite">
<p align="justify">Hello Guys, I have come up with an interesting article on enabling/disabling the out of box ribbon items in CRM 2011. This article demonstrates the order entity where I would be disabling the &ldquo;Create invoice button&rdquo; based on a condition in jscript. The script returns &ldquo;false&rdquo; or &ldquo;true&rdquo; value.</p>
<h3>Before we get started, here&rsquo;s the list of requirements with which you need to be familiar:</h3>
<ul>
<li>Microsoft Visual Studio 2010 / Notepad ++ (or XML editor)</li>
<li>MS CRM SDK 2011</li>
<li>Jscript</li>
</ul>
<h3>Walk-through</h3>
<ul>
<li>Go to settings &gt; customizations &gt; solutions</li>
<li>Click &ldquo;new&rdquo; add new solution</li>
<li>Provide a valid name, publisher and your version number</li>
<li>Save the solution</li>
<li>Add the existing &ldquo;Order&rdquo; entity.</li>
<li>Save &amp; close the dialog</li>
<li>Export the solution</li>
<li>Unzip the archive and open the customization.xml file</li>
<li>Search for &ldquo;RibbonDiffXML&rdquo; tag under the order entity</li>
<li>Add the following code</li>
</ul>
<p><img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/5074.code1.png" /></p>
<ul>
<li>Save and close the file</li>
<li>Zip all the three files Customization.xml, [Content_Types].xml and solution.xml</li>
<li>Go to CRM application</li>
<li>Go to settings &gt; customizations &gt; customize the system &gt; web resources</li>
<li>Click to add &ldquo;new&rdquo; web resource, provide a name &ldquo;new_CustomRule.js&rdquo;</li>
<li>Add the following code</li>
</ul>
<p class="sourceJs">function HideExisting()<br /> {<br /> return false;<br /> }</p>
<ul>
<li>Import the customizations</li>
<li>Publish the customizations</li>
<li>You can see the changes reflected</li>
</ul>
<p>You can download the solution below.</p>
<h3>Additional Information:</h3>
<ul>
<li>System Id of the ribbon: SDKresourcesexportedribbonxmlsalesorderribbon.xml</li>
<li>Define Ribbon Enable Rules <a href="http://msdn.microsoft.com/en-us/library/gg334682.aspx">http://msdn.microsoft.com/en-us/library/gg334682.aspx</a></li>
</ul>
<p>Cheers,<br /> Apurv</p>
</div>
<p><a href="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Components.PostAttachments/00/10/29/10/15/Export_Order_1_0_Final.zip">Export_Order_1_0_Final.zip</a></p>
