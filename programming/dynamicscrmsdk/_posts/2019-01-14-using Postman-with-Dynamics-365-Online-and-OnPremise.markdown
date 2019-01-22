---
layout: post
title:  "Using Postman with Dynamics 365 Online and OnPremise"
date:   2019-01-14 10:40:00 -0600
PostDate: 2019-01-14
author: apurvghai
ag_categories: Dynamics CRM SDK
lang: Postman, General
---

Postman is the most popular tool today to troubleshoot and write sample code for testing. Postman offers variety of programming options as well. I love to use variables option which is easy to then show the data.

There are different ways to use Postman with Online vs OnPremise system


##### Setup Postman

Follow instructions to setup your postman

- Download [Postman](https://www.getpostman.com/downloads/)
- Create an environment, let's say *CrmOnline*. See [instructions](https://learning.getpostman.com/docs/postman/environments_and_globals/manage_environments/)
- Once you've your environment setup, it's time to create variables. See [instructions](https://learning.getpostman.com/docs/postman/environments_and_globals/manage_environments/#editing-an-active-environment)

##### Prerequisites
- Setup your application Id in Azure (Type: WebApp/WebAPI)
- Assign permissions to *Dynamics CRM Online* - Delegated Permissions

#### Instructions

[Follow these instructions](https://learning.getpostman.com/docs/postman/sending_api_requests/requests/#form-data) to *Post* `x-www-form-urlencoded`. You can use the below parameters

<i class="post">POST</i> https://login.microsoftonline.com/your-azure-tenantid/oauth2/token

##### CRM Online - Server to Server authentication


```
grant_type: client_credentials
client_id: {client_id}
client_secret: {client_secret}
resource: {resourceurl}
```

##### CRM Online - User Password authentication


```
grant_type: password
client_id: {client_id}
client_secret: {client_secret}
resource: {resourceurl}
username: {username}
password: {password}
```

Navigate to Tests tab, paste below code:

```
var json = JSON.parse(responseBody);
postman.setEnvironmentVariable("BearerToken", json.access_token);
```



This will return [JWT](https://jwt.io/introduction/) Token which can be decoded from [this](https://jwt.ms) site and will be stored in your variables.


*Important Note* Create application user in CRM Online. Follow these [instructions](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/admin/create-users-assign-online-security-roles#create-an-application-user)


###### CRM WebAPI Operations
Now that you know how to perform POST or GET Operations. I will now demostrate how to use JWT token and make CRM Web API.

- Select GET Operation
- Use this URL: https://demo.crm.dynamics.com/data/api/v9.1/WhoAmI
- In the Headers tab, write the following
- *Use Bulk edit option*


`Authorization: Bearer {BearerToken}`

Hit __send__ and enjoy your results
