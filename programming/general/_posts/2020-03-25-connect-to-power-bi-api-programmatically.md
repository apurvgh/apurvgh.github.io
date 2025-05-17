---
layout: post-default
title: Connect to Power BI API Programmatically
date: 2020-03-25 18:30:49 +0000
PostDate: 2020-03-25
author: apurvghai
category: general
category_title: Rest API
lang: C#
---

Hello everyone!

In this article, we'll explore how to integrate with the Power BI REST API, focusing on authentication and programmatic access. Whether you're a developer, data analyst, or IT professional, this guide will help you take the right steps to connect securely to the Power BI service. The concepts here are language-agnostic, so you can adapt them to your preferred programming environment.

---

## Prerequisites

Before you begin, ensure you have the following:

- Postman (for testing API requests)
- Basic understanding of OAuth2
- Access to the Azure Portal

---

## Step 1: Register an Application in Azure and Assign Permissions

To interact with the Power BI API, you must first register an application in Azure Active Directory and grant it the necessary permissions.

- **Register your application:**  
  Follow the [official Microsoft guide](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app) to register your app.

- **Assign API permissions:**  
  The permissions you need depend on the API endpoints you intend to use. For example, to query workspaces, assign either `Workspace.ReadWrite.All` or `Workspace.Read.All`.

  You can find a [complete list of Power BI API permissions here](https://docs.microsoft.com/en-us/rest/api/power-bi/).

> **Note:**  
> Most Power BI API permissions require delegated access.

---

## Step 2: Obtain an Access Token Using Postman

You can authenticate using either the Authorization Code flow or Client Credentials. For demonstration, we'll use Postman with the Resource Owner Password Credentials (ROPC) grant type.

**Request:**

```
POST https://login.microsoftonline.com/<your-tenant-id>/oauth2/token
Body (x-www-form-urlencoded):
    grant_type: password
    username: <your-username>
    password: <your-password>
    client_id: <your-application-id>
    client_secret: <your-client-secret>
    resource: https://analysis.windows.net/powerbi/api
    scope: Workspace.Read.All User.Read
```

Once you send the request, you'll receive an access token.

> **Tip:**  
> If you use ADAL libraries, the `grant_type=password` flow is not supported. Instead, use the User Credential class.

Use the access token to make authorized API calls.

**Example API Call:**

```
GET https://api.powerbi.com/v1.0/myorg/admin/groups?$top=1
Headers:
    Authorization: Bearer <your-access-token>
```

**Sample Response:**

```json
{
  "@odata.context": "http://wabi-us-central-a-primary-redirect.analysis.windows.net/v1.0/myorg/admin/$metadata#groups",
  "@odata.count": 2,
  "value": [
    {
      "id": "F769F2BB-9825-4B11-8705-998FAF1F8B22",
      "isReadOnly": false,
      "isOnDedicatedCapacity": false,
      "capacityMigrationStatus": "",
      "type": "Workspace",
      "state": "Active",
      "name": "Sample Workspace"
    }
  ]
}
```

---

## Step 3: Authenticate Using PowerShell (Non-Interactive / Service Principal)

For server-to-server scenarios, use an Azure Service Principal for non-interactive authentication.

### Prerequisites

- **Create a client secret** for your Azure application.  
  [How to create a service principal](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal)

- **Enable service principal access in Power BI:**  
  - Go to the Power BI Admin Portal.
  - Navigate to Tenant Settings.
  - Enable "Allow service principal to use Power BI APIs".
  - Add your app to the required security group.

  [Read more about using service principals with Power BI](https://powerbi.microsoft.com/en-us/blog/use-power-bi-api-with-service-principal-preview/)

- **Authorize your app to access workspaces:**  
  Use the Power BI PowerShell module to assign roles.

### Example PowerShell Script

```powershell
# Install Power BI Module (if not already installed)
Install-Module MicrosoftPowerBIMgmt.Workspaces

# Login interactively
Login-PowerBI

# Service Principal Object ID
$SPObjectId = '36......'

# Get the workspace
$pbiWorkspace = Get-PowerBIWorkspace -Name "<Name of workspace>"

# Confirm workspace details
Write-Host $pbiWorkspace

# Add Service Principal as Admin or Member
Add-PowerBIWorkspaceUser -Id $pbiWorkspace.Id -AccessRight Admin -PrincipalType App -Identifier $SPObjectId
```

---

## Step 4: Test Your Integration

You can now test your setup using the following PowerShell script, which demonstrates how to obtain a token and trigger a dataset refresh:

```powershell
# Login to Azure CLI
az login

# Retrieve secret from Azure Key Vault
$secretData = az keyvault secret show --vault-name '<your-keyvault-name>' --name '<your-secret-name>' | ConvertFrom-Json | Select value

$ClientId = "<Your Client ID/ApplicationId>"
$ClientSecret = $secretData.value
$loginURL = "https://login.microsoftonline.com"
$tenantdomain = "<your-tenant-domain>"
$scope1 = "https://analysis.windows.net/powerbi/api/.default"
$clientSecret = (ConvertTo-SecureString $ClientSecret -AsPlainText -Force)
$msal = Get-MsalToken -clientID $ClientId -clientSecret $clientSecret -tenantID $tenantdomain -Scopes $scope1
$accessToken = $msal.AccessToken

# Prepare headers
$AuthHeader = @{
  'Content-Type'  = 'application/json'
  'Authorization' = "Bearer $($accessToken)"
}

# Set Power BI API URL
$URI = "https://api.powerbi.com/v1.0/myorg/groups/<YourGroupId>/datasets/<YourDataSetId>/refreshes"

# Trigger dataset refresh
Invoke-RestMethod -Uri $URI -Headers $AuthHeader -Method POST
```

---

## Conclusion

With these steps, you should be able to authenticate and interact with the Power BI REST API programmatically, whether using Postman, PowerShell, or your preferred programming language. If you have any questions or want to share your experience, feel free to leave a comment!

Happy coding!
