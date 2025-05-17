---
layout: post-default
title:  Getting started with Dynamics 365 CRM SDK in C#
date:   2019-01-14 11:00:00 -0600
PostDate: 2019-01-14
author: apurvghai
category: dynamicscrmsdk
category_title: Dataverse SDK
lang: C#
---

Getting started with CRM SDK has been this simple. CRM now offers you variety of options to connect with. You can choose between XRM Tooling Connector, Standard Core assemblies, or simply use OAuth with Web API. This article will talk about CRM SDK using [late binding](https://en.wikipedia.org/wiki/Late_binding) approach. 

##### CRM Tooling Connector
[Tooling connector](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/xrm-tooling/use-crmserviceclient-constructors-connect) is the easiest form of SDK and is very simple to implement. Let's see how to use it

- Create a visual studio console application project in *Csharp*
- Use the nuget manager to restore [Xrm tooling connector](https://www.nuget.org/packages/Microsoft.CrmSdk.XrmTooling.CoreAssembly/). See [sample](https://www.nuget.org/packages/Microsoft.CrmSdk.XrmTooling.CoreAssembly/) if you are not sure on how to use nuget.
- Write the following code


```csharp

cons string Url = "https://demo.crm.dynamics.com";
cons string Username = "user@domain.onmicrosoft.com"; //You can use federated too
cons string Password = "*********"
var connectionString = $"Url={Url}; Username={Username}; Password={Password}; 
     AuthType=Office365";
var client = new CrmServiceClient(connectionString);
if(client.IsReady) {
    Console.Log("Your connection has been established");
}
else {
    Console.Log("An error ocurred");
}

```
You can review the list of connection string available in this [document](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/xrm-tooling/use-connection-strings-xrm-tooling-connect).