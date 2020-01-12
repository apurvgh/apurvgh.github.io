---
layout: post-default
title: CRM Early Bound Entity Classes in Code Vs. .NET Traditional LINQ (Entity Framework)
date: 2013-4-26 16:3:0 - 0600
PostDate: 2013-4-26
author: apurvghai
ag_categories: Dynamics CRM SDK
lang: C#
---
<p class="body">Hello Folks,</p>
<p>I have been always writing some basic sort of examples on CRM scripting / SDK. This time I have brought down this to .NET core feature called &ldquo;Entity Framework&rdquo; versus &ldquo;CRM SDK&rdquo;. <br /> This article is useful is people who are quite familiar with LINQ but are new to CRM entity classes. <br /> <br /> Let&rsquo;s take a look at the code comparisons below:</p>
<p><br /> .NET Entity Framework Example: <br /> SQL Table: Student<br /><br /></p>
<table style="border:1px solid #000;" border="1" cellspacing="3" cellpadding="3">
<tbody>
<tr>
<td>id</td>
<td>int</td>
</tr>
<tr>
<td>name</td>
<td>varchar(300)</td>
</tr>
<tr>
<td>marks</td>
<td>int</td>
</tr>
</tbody>
</table>
<p>Now when we generate the .NET Entity Model using .edmx file. We will see the classes have been regenerated with relevant properties as below</p>
<p class="sourceCode"><span class="functionName">public int</span> id {<span class="functionName">&nbsp;&nbsp;get</span>;<span class="functionName">&nbsp;&nbsp;set&nbsp;</span>;}<br /> <span class="functionName">public string</span> name {<span class="functionName">&nbsp;&nbsp;get</span>;<span class="functionName">&nbsp;&nbsp;set&nbsp;</span>;}<br /> <span class="functionName">public int</span> marks {<span class="functionName">&nbsp;&nbsp;get</span>;<span class="functionName">&nbsp;&nbsp;set&nbsp;</span>;}</p>
<p>When we start to use this in LINQ implementation, we would generally use Students object and receive the data.</p>
<p><span class="functionName">var</span> context = <span class="functionName">new</span> <span class="className">EntityFrameWorkEntities</span>(); <br /> <span class="functionName">var</span> query = (<span class="functionName">from</span> o in context.Students <span class="functionName">select o</span> );</p>
<p>This will result into complete rows of Student Table. Now let&rsquo;s have a look at the way we would write CRM classes using LINQ. First and foremost we would need to generate classes using CrmSvcUtil.exe.</p>
<p>Below is the screen shot of CrmSvcUtil Screen shot:</p>
<p><img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/45/90/0878.crmsvc.png" alt="" border="0" /></p>
<p>Consider a sample entity called &ldquo;new_Student&rdquo; with similar fields like we took in above example. <br /> <br /> Here&rsquo;s what it would look like: <br /> <br />CRM SQL table: new_student</p>
<table style="border:1px solid #000;" border="1" cellspacing="3" cellpadding="3">
<tbody>
<tr>
<td>new_id</td>
<td>int</td>
</tr>
<tr>
<td>new_name</td>
<td>nvarchar(300)</td>
</tr>
<tr>
<td>new_marks</td>
<td>int</td>
</tr>
</tbody>
</table>
<p>Here&rsquo;s the LINQ implementation:</p>
<p class="sourceCode"><span class="className">IOrganizationService</span> service = ReturnCrmService(); <br /> <span class="functionName">using</span> (<span class="functionName">var</span> context = <span class="functionName">new</span> <span class="functionName">CrmContext</span>(service)) <br /> { <br /> <span class="functionName">var</span> query = (<span class="functionName">from</span> o in context.AccountSet <span class="functionName">select new</span>{ o.Name }.Name); <br /> <span class="functionName">foreach</span> (<span class="functionName">string</span> name in query) <br /> { <br /> <span class="className">Console</span>.WriteLine(<span class="functionName">string</span>.Format("Account Name: {0}", name)); <br /> } <br /> <span class="className">Console</span>.ReadKey(); <br /> }</p>
<p>By default all CRM entities are post pended with the word &ldquo;Set&rdquo;. This is how you can identify the entity names. I have attached the working CRM Project for your quick reference.</p>
<p>Hope this information was helpful.</p>
<p>Cheers, <br /> Apurv</p>
<p><a href="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Components.PostAttachments/00/10/41/44/12/Program.zip">Program.zip</a></p>
