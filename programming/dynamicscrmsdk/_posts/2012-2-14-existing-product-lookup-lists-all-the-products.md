---
layout: post-default
title: Existing product lookup lists all the products
date: 2012-2-14 23:13:0 - 0600
PostDate: 2012-2-14
author: apurvghai
ag_categories: Dynamics CRM SDK
lang: C#
---
<div class="blogSite">
<p>This article talks about a parent look view which is never visible after the &ldquo;Display search box in lookup diaglog&rdquo; is checked while customizing Opportunity Product Entity in CRM 2011. There is an easy workaround to get back to old view. I have mentioned the ways you see this happening and a tweak in customization which will revert this back.</p>
<h3>How can I see a different view?</h3>
<ol type="1">
<li>Go to Settings &gt; Customizations &gt; Customize the system &gt; Entities &gt; Opportunity Product &gt; Forms &gt; Main Form &gt; Existing Product</li>
<li>Select "Change Properties"</li>
<li>Go to General Tab and scroll down until you find &ldquo;Display this is search box&rdquo;</li>
<li>Check that setting and save the entity following to publish it.</li>
<li>Now go to, opportunities entity</li>
<li>Add a price list and continue adding an existing product</li>
<li>A screen will appear to choose existing product.</li>
<li>The moment you click lookup icon, you will see that the name of view has changed to &ldquo;Product Lookup View&rdquo;</li>
</ol>
<h3>How do I revert to old view?</h3>
If you&rsquo;re thinking that you can just go and uncheck. No then that&rsquo;s not a solution. We will need to revert through a customization tweak. The reason for this and by now CRM has reset the view id. Hence it shows a different view. <br />The following are steps to achieve that<ol type="1">
<li>Go to Settings &gt; Solutions</li>
<li>Add new Solution and provide a valid name to it.</li>
<li>Save the solution.</li>
<li>Now add existing entity, &ldquo;Opportunity Product&rdquo;</li>
<li>Export the solution as unmanaged</li>
<li>Open the unmanaged solution in notepad and locate the following:</li>
<li><img width="535" src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/3443.Lookupview.png" border="0" /></li>
<li>Replace <span class="sourceCode">{8BA625B2-6A2A-4735-BAB2-0C74AE8442A4}</span> with <span class="sourceCode">{BCC509EE-1444-4A95-AED2-128EFD85FFD5}</span></li>
<li>Save the changes</li>
<li>Zip the files and import the solution back to your environment</li>
<li>Publish the customizations</li>
<li>Refresh the CRM in your browser</li>
<li>You will see the correct view.</li>
</ol>You can always check the correct GUID values in the database. <br /><br /> Happy customizing :) <br /><br /> Cheers,<br /> Apurv</div>
