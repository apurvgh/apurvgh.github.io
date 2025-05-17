---
layout: post-default
title: Formatting Telephone Numbers (CRM 2011)
date: 2011-9-21 12:48:0 - 0600
PostDate: 2011-9-21
author: apurvghai
category: dynamicscrmsdk
category_title: Dataverse SDK
ag_original_link: https://learn.microsoft.com/en-us/archive/blogs/apurvghai/formatting-telephone-numbers-crm-2011
lang: C#
---
<div class="blogSite">
<p>This post talks about formatting phone number is US style. The code already available in CRM v4 SDK Guide, however I have re-written the same thing in CRM v2011 model.</p>
<p>It only part of code which is different here in the usage of context class available in XRM pattern.</p>
<div class="sourceJs">function validatePhone()<br /> {&nbsp;<br /> <br /> var phone = Xrm.Page.data.entity.attributes.get("telephone1");<br /> var sTmp = phone.getValue().replace(/[^0-9]/g, "");<br /> phoneRegex = /^d{10}$/;&nbsp;<br /> <br /> if(!sTmp.match(phoneRegex))<br /> {<br /> alert("Phone must contain 10 numbers.") ;<br /> }<br /> else<br /> {<br /> switch(sTmp.length)<br /> {<br /> case "4105551212".length:<br /> phone.setValue( "(" + sTmp.substr(0, 3) + ") " + sTmp.substr(3, 3) + "-" + sTmp.substr(6, 4));<br /> break;<br /> <br /> case "5551212".length:<br />phone.setValue= sTmp.substr(0, 3) + "-" + sTmp.substr(3, 4);<br /> break;<br /> <br /> }<br /> }&nbsp;<br /> }</div>
<br /><br /> <b>References</b><br /><ol>
<li><a href="http://msdn.microsoft.com/en-us/library/gg328474.aspx" target="_blank">Formatting Phone Numbers in CRM v4</a></li>
</ol></div>
