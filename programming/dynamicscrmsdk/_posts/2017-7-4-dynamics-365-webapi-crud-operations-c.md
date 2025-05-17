---
layout: post-default
title: Dynamics 365 – WebAPI CRUD Operations (C#)
date: 2017-7-4 15:29:28 - 0600
PostDate: 2017-7-4
author: apurvghai
category: dynamicscrmsdk
category_title: Dataverse SDK
ag_original_link: https://learn.microsoft.com/en-us/archive/blogs/apurvghai/dynamics-365-webapi-crud-operations-c
lang: C#
---
I've added this post to showcase the CRUD operations in C# through WebAPI endpoint. You must have working experience in below areas: –
<ul>
 	<li>Program using C#</li>
 	<li>Dynamics 356 Online.</li>
 	<li>Visual Studio</li>
 	<li>Conceptual knowledge of ODATA Operations.</li>
 	<li>Using ADAL + OAuth</li>
</ul>
All the operations below require access token to be passed as a header hence it's important to know how to use ADAL (Active Directory Azure Authentication Library).

WebAPI (or REST) endpoints are very easy to program as they just require you to know the authentication protocol then it's all about sent HTTP Requests with respective OData Filters. Each operation requires a unique set of URLs to be passed. Below you see a sample for creating a record. You can find completed repository on <a href="https://github.com/apurvgh/Dynamics365Samples/blob/master/Dyn365Samples/WebAPISamples/WebAPIHelper.cs">GitHub</a>.
<pre>        /// &lt;summary&gt;
        /// This function can create any record type using Json Entity Objects
        /// &lt;/summary&gt;
        /// &lt;param name="entityName"&gt;&lt;/param&gt;
        /// &lt;param name="entityObject"&gt;&lt;/param&gt;
        /// &lt;returns&gt;&lt;/returns&gt;
        public async Task CreateEntityRecords(string entityName, JObject entityObject)
        {
            using (httpClient = CreateDynHttpClient(AccessToken, entityName))
            {
                HttpRequestMessage createHttpRequest = new HttpRequestMessage(HttpMethod.Post, BaseOrganizationApiUrl + "/api/data/v8.1/" + entityName);
                createHttpRequest.Content = new StringContent(entityObject.ToString(), Encoding.UTF8, "application/json");
                var response = await httpClient.SendAsync(createHttpRequest);
                response.EnsureSuccessStatusCode();
                if (response.StatusCode == HttpStatusCode.NoContent)
                {
                    // Do something with response. Example get content:
                    Console.WriteLine("The record was created successfully.");
                    Console.ReadKey();
                }
            }
        }
</pre>
&nbsp;

Happy WebAPI'ing :)

Cheers,
