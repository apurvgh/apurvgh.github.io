---
layout: post-default
title:  "Trigger logic apps using Powershell or C#"
date:   2019-12-29 17:53:00 -0600
PostDate: 2019-12-29
author: apurvghai
category: logicapps
category_title: Logic Apps
lang: JSON, Powershell, C#
---

I was recently doing a project at work where I needed to trigger logic apps inside Build Pipeline. So I chose powershell as my weapon here. Powershell tasks are readily available inside the [DevOps Pipeline](https://azure.microsoft.com/en-us/services/devops/pipelines/). If you are interested in reading more about how to use powershell inside devops please read [this post](/programming/azuredevops/2019/06/21/customize-your-build-number.html).

### Logic apps Setup

 In order for us to trigger logic apps, the app must be designed to accept _HTTP_ requests. To learn more about it, you can follow this link to [official documentation](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-http-endpoint). 

 Let's pretend that we have following `JSON` payload

 ```json
 {
   "FirstName": "John",
   "LastName": "Doe",
   "Title": "Software Engineer",
   "Email": "john.doe@something.com"
 }
 ```
 
 Now we need to generate the schema using logic apps. The screen below demonstrates that

 <br/><a data-flickr-embed="true" href="https://www.flickr.com/photos/186248049@N06/49295492938/in/dateposted-public/" title="http-logic-trigger"><img src="https://live.staticflickr.com/65535/49295492938_949ea665df_z.jpg" width="640" height="382" alt="http-logic-trigger"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script><br/>

Once we have completed the the initial setup, let's go to the next step and execute our powershell. 

### Powershell script

 The script below is fairly simple as all it does is sends HTTP request to logic apps URL with `JSON` data. 

  ```powershell
 
 $data =@{
    "FirstName"= "John"
    "LastName"= "Doe"
    "Title"= "Software Engineer"
    "Email"= "john.doe@something.com"
 }

 $json = $data | ConvertTo-Json
 
 $LogicAppsUrl = "https://yourlogicappsurl"
 $LogicAppInfo = Invoke-WebRequest -Uri $LogicAppsUrl -Headers @{
    "Content-Type" = "application/json"
 } -Method Post -Body $json  -UseBasicParsing

 Write-Host "Trigger Complete"
 ```

### C# Script

 ```cs

 public class LogicAppsModel {
    public string FirstName {get; set;}
    public string LastName {get; set;}
    public string Title {get; set;}
    public string Email {get; set;}
 }

 public async Task SendRequestAsync() {
    var logicAppsModel = new LogicAppsModel();
    var logicAppsUrl = "yoururl";
    var client = new HttpClient();
    var request = new HttpRequestMessage(HttpMethod.Post, logicAppsUrl);
    request.Content = new StringContent(JsonConvert.Serialize(logicAppsModel), Encoding.UTF8, "application/json");
    var response =  await client.SendAsync(request);
    Console.WriteLine("Trigger Complete");
 }
 ```

 **Note**: Make sure to include SAS signature in your logic apps url. `https://<yoururl>/workflows/yourworkflowid/triggers/request/paths/invoke?api-version=2016-10-01&sp=/triggers/request/run&sv=1.0&sig=yoursassignature`

 Hope you enjoyed reading it. Please leave your comments below.

