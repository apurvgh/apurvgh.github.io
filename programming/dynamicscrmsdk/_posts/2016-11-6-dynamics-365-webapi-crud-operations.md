---
layout: post-default
title: Dynamics 365 - WebAPI Crud Operations (JavaScript)
date: 2016-11-6 15:1:1 - 0600
PostDate: 2016-11-6
author: apurvghai
category: dynamicscrmsdk
category_title: Dataverse SDK
ag_original_link: https://learn.microsoft.com/en-us/archive/blogs/apurvghai/dynamics-365-webapi-crud-operations
lang: JavaScript
---
Hey Guys,

It's been a while since I've posted. I know for sure this is late. I think it would be helpful to see basics items on WebAPI (using JavaScript). The article today talks about the CRUD (Create, update, read and delete) operations using WebAPI endpoints.

You must have working experience in below areas: -
<ul>
 	<li>Code using Java scripting</li>
 	<li>Dynamics 356 customizations or CRM online 2015,2016, 2016 SP1 Web Resources</li>
 	<li>Visual Studio</li>
 	<li>Conceptual knowledge of ODATA Operations.</li>
</ul>
I've created HTML web resource in Dynamics 365 and a script Library to add below functions. I will although not explain how to create web resources. The source code is now available on GitHub. The demo covers the following scenario:
<ul>
 	<li>Searching an existing account, if found it will show an alert with a count of accounts found
<ul>
 	<li>If not found, then it will ask you if you want to create one.</li>
</ul>
</li>
</ul>
//Creating Account:

<a href="https://msdnshared.blob.core.windows.net/media/2016/11/create-webapi-js.png"><img width="862" height="377" class="alignnone wp-image-525 size-full" alt="create-webapi-js" src="https://msdnshared.blob.core.windows.net/media/2016/11/create-webapi-js.png" /></a>

//Search or Retrieve Account

<img width="1321" height="519" class="alignnone wp-image-536 size-full" alt="retrieve-webapi-js" src="http://apurvghai.files.wordpress.com/2016/11/retrieve-webapi-js.png" />

There are many types of filters which can be used. A list is available at OASIS Open Source Documentation 4.0: <a href="http://docs.oasis-open.org/odata/odata/v4.0/errata02/os/complete/part1-protocol/odata-v4.0-errata02-os-part1-protocol-complete.html#_The_$filter_System">http://docs.oasis-open.org/odata/odata/v4.0/errata02/os/complete/part1-protocol/odata-v4.0-errata02-os-part1-protocol-complete.html#_The_$filter_System</a>

<a href="http://apurvghai.files.wordpress.com/2016/11/odata-filters.png"><img width="922" height="492" class="alignnone wp-image-535 size-full" alt="odata-filters" src="http://apurvghai.files.wordpress.com/2016/11/odata-filters.png" /></a>

<span>//Update existing Account:</span>

<a href="http://apurvghai.files.wordpress.com/2016/11/update-webapi-js.png"><img width="889" height="345" class="alignnone wp-image-546" alt="update-webapi-js" src="http://apurvghai.files.wordpress.com/2016/11/update-webapi-js.png" /></a>

Full source of the demo is available on GitHub:

Script Library: <a href="https://github.com/apurvgh/Dynamics365Samples/blob/master/new_webapi.lib.js">https://github.com/apurvgh/Crm_Sdk_Samples/blob/master/new_webapi.lib.js</a>

HTML WebResource: <a href="https://github.com/apurvgh/Crm_Sdk_Samples/blob/master/new_webapidemo.html">https://github.com/apurvgh/Crm_Sdk_Samples/blob/master/new_webapidemo.html</a>

Thank you for spending time. :)

-Apurv
