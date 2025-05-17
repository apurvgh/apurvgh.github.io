---
layout: post-default
title: Troubleshooting and developing oauth enabled applications with Dynamics 365
date: 2016-12-11 16:51:57 - 0600
PostDate: 2016-12-11
author: apurvghai
category: dynamicscrmsdk
category_title: Dataverse SDK
ag_original_link: https://learn.microsoft.com/en-us/archive/blogs/apurvghai/troubleshooting-and-developing-oauth-enabled-applications-with-dynamics-365
lang: C#
---
OAuth applications can be developed using Dynamics 365 Online and OnPremise versions. Let's see these in more details:

<strong>HOW DOES OAUTH WORK IN GENERAL</strong>

<a href="http://apurvghai.files.wordpress.com/2016/12/oauth-flow.png"><img src="https://msdnshared.blob.core.windows.net/media/2016/12/oAuth-flow-1024x597.png" alt="oauth-flow" width="879" height="512" class="aligncenter size-large wp-image-645" /></a>

&nbsp;
<ul class="lf-text-block lf-block">
 	<li>The <strong>Authorization Server</strong> is the v2.0 endpoint. It is responsible for ensuring the user's identity, granting and revoking access to resources, and issuing tokens. It is also known as the identity provider - it securely handles anything to do with the user's information, their access, and the trust relationships between parties in an flow.</li>
 	<li>The <strong>Resource Owner</strong> is typically the end-user. It is the party that owns the data, and has the power to allow third parties to access that data, or resource.</li>
 	<li>The <strong>OAuth Client</strong> is your app, identified by its Application Id. It is usually the party that the end-user interacts with, and it requests tokens from the authorization server. The client must be granted permission to access the resource by the resource owner.</li>
 	<li>The <strong>Resource Server</strong> is where the resource or data resides. It trusts the Authorization Server to securely authenticate and authorize the OAuth Client, and uses Bearer access_tokens to ensure that access to a resource can be granted.</li>
</ul>
<span>You can read the complete detail on </span><a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-v2-protocols">Microsoft Azure Documentation</a><span>:</span>

<strong>UNDERSTANDING THE FLOW OF YOUR APPLICATION</strong>

Now in our scenario we will register our c# application in azure management portal. <a target="_blank" href="https://blogs.msdn.microsoft.com/apurvghai/2016/12/03/configure-azure-and-create-crm-apps-using-oauth-webapi/">Read more to create apps in C#</a>. Let's go and see how will this work in real time. I will demonstrate this through an image

<a href="http://apurvghai.files.wordpress.com/2016/12/realtime-flow.png"><img src="https://msdnshared.blob.core.windows.net/media/2016/12/realtime-flow-1024x683.png" alt="realtime-flow" width="879" height="586" class="aligncenter wp-image-675 size-large" /></a>

Let's dissect to understand the parameter values:
<ul>
 	<li>Client ID : 32-Digit Guid registered in Azure</li>
 	<li>Resource Url: Fully Qualified Dynamics 365 Url (ex: orgname.crm.dyamics.com)</li>
 	<li>Oauth Endpoint: Fully Qualified Azure Oauth endpoint url (should be retrieved dynamically). See this <a target="_blank" href="https://blogs.msdn.microsoft.com/apurvghai/2016/12/03/configure-azure-and-create-crm-apps-using-oauth-webapi/">post</a>: <em>Sample</em>: https://login.windows.net/&lt;tenantId&gt;/oauth2/authorize?</li>
</ul>
Passing these to ADAL Library will prompt user with Office 365 Logon page.

You can visit Developer Center to see how to obtain Dynamics 365 Endpoint Urls:<a href="https://www.microsoft.com/en-us/dynamics/crm-customer-center/view-or-download-developer-resources.aspx"> https://www.microsoft.com/en-us/dynamics/crm-customer-center/view-or-download-developer-resources.aspx</a>.

Each function written in ADAL and when used in c# application is an Async function. Which means you will be using Task (<em><strong>System.Threading.Tasks.Task</strong></em>) and if the application fails to connect or authenticate you will receive "<em>Task has been canceled</em>" exception. Then you will need to expand the exception dialog in Visual Studio to see the inner exception.

<strong>See example:</strong>
<pre>Program app = new Program();
 Task.WaitAll(Task.Run(async () =&gt; await app.CreateMyReordsAsync())); 

Function definition

 async Task CreateMyReordsAsync()
        {
            WebApiOperationHelper apihelper = new WebApiOperationHelper();
            apihelper.BaseOrganizationApiUrl = "https://org.api.crm.dynamics.com";
            apihelper.ObtainOAuthToken(); <strong>//Let's say this step failed.</strong>

            if (!(string.IsNullOrEmpty(apihelper.AccessToken)))
            { //Success go }</pre>
<strong>UNDERSTANDING FIDDLER REQUESTS FOR YOUR APPLICATION</strong>

Let's see the complete application flow in fiddler
<table>
<tbody>
<tr>
<td><b><span>#</span></b><span> </span></td>
<td><b><span>Result</span></b><span> </span></td>
<td><b><span>Protocol</span></b><span> </span></td>
<td><b><span>Host</span></b><span> </span></td>
<td><b><span>URL</span></b><span> </span></td>
<td><b><span>Body</span></b><span> </span></td>
<td><b><span>Caching</span></b><span> </span></td>
<td><b><span>Content-Type</span></b><span> </span></td>
</tr>
<tr>
<td><b><span>2</span></b><span> </span></td>
<td><span>200</span><span> </span></td>
<td><span>HTTP</span><span> </span></td>
<td><span>Tunnel to</span><span> </span></td>
<td><span>orgname</span><span>.crm.dynamics.com:443</span><span> </span></td>
<td><span>0</span><span> </span></td>
<td><span> </span></td>
<td><span> </span></td>
</tr>
<tr>
<td><b><span>3</span></b><span> </span></td>
<td><span>401</span><span> </span></td>
<td><span>HTTPS</span><span> </span></td>
<td><span>orgname</span><span>.crm.dynamics.com</span><span> </span></td>
<td><span>/</span><span>api</span><span>/data/v8.1</span><span> </span></td>
<td><span>49</span><span> </span></td>
<td><span> </span></td>
<td><span>text/html</span><span> </span></td>
</tr>
<tr>
<td><b><span>4</span></b><span> </span></td>
<td><span>200</span><span> </span></td>
<td><span>HTTP</span><span> </span></td>
<td><span>Tunnel to</span><span> </span></td>
<td><span>login.windows.net:443</span><span> </span></td>
<td><span>0</span><span> </span></td>
<td><span> </span></td>
<td><span> </span></td>
</tr>
<tr>
<td><b><span>5</span></b><span> </span></td>
<td><span>302</span><span> </span></td>
<td><span>HTTPS</span><span> </span></td>
<td><span>login.windows.net</span><span> </span></td>
<td><span>/&lt;tenantid&gt;/oauth2/</span><span>authorize?resource</span><span>=https%3A%2F%2Forgname.</span><span>crm</span><span>.dynamics.com%2F&amp;client_id=&lt;clientid&gt;&amp;response_type=code&amp;redirect_uri=http%3A%2F%2Fgo%2F&amp;client-request-id=4e9ee0d9-516a-455a-9f5d-373eea250208&amp;prompt=login&amp;x-client-SKU=.NET&amp;x-client-Ver=2.22.0.0&amp;x-client-CPU=x64&amp;x-client-OS=Microsoft+Windows+NT+6.2.9200.0</span><span> </span></td>
<td><span>391</span><span> </span></td>
<td><span>private</span><span> </span></td>
<td><span>text/html; charset=utf-8</span><span> </span></td>
</tr>
<tr>
<td><b><span>6</span></b><span> </span></td>
<td><span>200</span><span> </span></td>
<td><span>HTTP</span><span> </span></td>
<td><span>Tunnel to</span><span> </span></td>
<td><span>login.microsoftonline.com:443</span><span> </span></td>
<td><span>0</span><span> </span></td>
<td><span> </span></td>
<td><span> </span></td>
</tr>
<tr>
<td><b><span>7</span></b><span> </span></td>
<td><span>200</span><span> </span></td>
<td><span>HTTPS</span><span> </span></td>
<td><span>login.microsoftonline.com</span><span> </span></td>
<td><span>/&lt;tenantId&gt;/oauth2/</span><span>authorize?resource</span><span>=https%3A%2F%2Forgname.</span><span>crm</span><span>.dynamics.com%2F&amp;client_id=&lt;clientid&gt;&amp;response_type=code&amp;redirect_uri=http%3A%2F%2Fgo%2F&amp;client-request-id=4e9ee0d9-516a-455a-9f5d-373eea250208&amp;prompt=login&amp;x-client-SKU=.NET&amp;x-client-Ver=2.22.0.0&amp;x-client-CPU=x64&amp;x-client-OS=Microsoft+Windows+NT+6.2.9200.0</span><span> </span></td>
<td><span>12,572</span><span> </span></td>
<td><span>no-cache, no-store; Expires: -1</span><span> </span></td>
<td><span>text/html; charset=utf-8</span><span> </span></td>
</tr>
<tr>
<td><b><span>8</span></b><span> </span></td>
<td><span>200</span><span> </span></td>
<td><span>HTTPS</span><span> </span></td>
<td><span>login.microsoftonline.com</span><span> </span></td>
<td><span>/common/instrumentation/</span><span>reportpageload</span><span> </span></td>
<td><span>264</span><span> </span></td>
<td><span>private</span><span> </span></td>
<td><span>application/json; charset=utf-8</span><span> </span></td>
</tr>
<tr>
<td><b><span>9</span></b><span> </span></td>
<td><span>200</span><span> </span></td>
<td><span>HTTPS</span><span> </span></td>
<td><span>login.microsoftonline.com</span><span> </span></td>
<td><span>/common/</span><span>userrealm</span><span>/?user=admin%40orgname</span><span>.</span><span>onmicrosoft</span><span>.com&amp;</span><span>api</span><span>-version=2.1&amp;stsRequest=rQIIAePiMNIzMtIz0DPQYjbUM7RSMTEzNk82S03UNU9JNNA1SU5J0k1MSjXUNTFISzUyM041NDYyLBLiErj5YJ5fVmCU6_xYc7tXqkwcqxi5MkpKCqz09dPz9XcwMl5gZHzByNjAxHiLid_fsbQkwwhE5BdlVqU-whB5xcSak5-emTeJmT8_ESSjByKT81NSVzErgowtBpqbWFBaVJaeoZdclKuXUpmXmJuZXKyXnJ-rv4lZxTDJzDI12dxUNy3ZAujWxDRD3cQ0U3PdVFMz8xSz1BRzYwvjXcwqFmYmyckGBom6SWmmqbomRgapQM-ZmumaG1qmGBsmpqYkp6TcYGa8wML4ioWHg0lATIJBgUGDxYDvBwvjIlagr1fyt-VacOp4zP37aKfB84cOp1j13YMrDBIrMgKMK42NK9LDsgu8LMIi_FyCykNyCi0j_LMzfavCPPw8SkotHW3NrQx3cSKFEwA1&amp;checkForMicrosoftAccount=true</span><span> </span></td>
<td><span>237</span><span> </span></td>
<td><span>private</span><span> </span></td>
<td><span>application/json; charset=utf-8</span><span> </span></td>
</tr>
<tr>
<td><b><span>10</span></b><span> </span></td>
<td><span>200</span><span> </span></td>
<td><span>HTTP</span><span> </span></td>
<td><span>Tunnel to</span><span> </span></td>
<td><span>2-edge-chat.</span><span>facebook</span><span>.com:443</span><span> </span></td>
<td><span>0</span><span> </span></td>
<td><span> </span></td>
<td><span> </span></td>
</tr>
<tr>
<td><b><span>11</span></b><span> </span></td>
<td><span>200</span><span> </span></td>
<td><span>HTTPS</span><span> </span></td>
<td><span>2-edge-chat.</span><span>facebook</span><span>.com</span><span> </span></td>
<td><span>/</span><span>pull?channel</span><span>=p_730503133&amp;seq=28&amp;partition=-2&amp;clientid=</span><span>4b5b6dbc</span><span>&amp;cb=gayo&amp;idle=320&amp;qp=y&amp;cap=8&amp;pws=fresh&amp;isq=9879&amp;msgs_recv=28&amp;uid=730503133&amp;viewer_uid=730503133&amp;sticky_token=4&amp;sticky_pool=ash3c07_chat-proxy</span><span> </span></td>
<td><span>28</span><span> </span></td>
<td><span>private, no-store, no-cache, must-revalidate</span><span> </span></td>
<td><span>application/json</span><span> </span></td>
</tr>
<tr>
<td><b><span>12</span></b><span> </span></td>
<td><span>302</span><span> </span></td>
<td><span>HTTPS</span><span> </span></td>
<td><span>login.microsoftonline.com</span><span> </span></td>
<td><span>/&lt;tenantid&gt;/login</span><span> </span></td>
<td><span>741</span><span> </span></td>
<td><span>no-cache, no-store; Expires: -1</span><span> </span></td>
<td><span>text/html; charset=utf-8</span><span> </span></td>
</tr>
<tr>
<td><b><span>13</span></b><span> </span></td>
<td><span>200</span><span> </span></td>
<td><span>HTTP</span><span> </span></td>
<td><span>Tunnel to</span><span> </span></td>
<td><span>login.windows.net:443</span><span> </span></td>
<td><span>0</span><span> </span></td>
<td><span> </span></td>
<td><span> </span></td>
</tr>
<tr>
<td><b><span>14</span></b><span> </span></td>
<td><span>200</span><span> </span></td>
<td><span>HTTPS</span><span> </span></td>
<td><span>login.windows.net</span><span> </span></td>
<td><span>/&lt;tenantid&gt;/oauth2/token</span><span> </span></td>
<td><span>3,148</span><span> </span></td>
<td><span>no-cache, no-store; Expires: -1</span><span> </span></td>
<td><span>application/json; charset=utf-8</span><span> </span></td>
</tr>
<tr>
<td><b><span>15</span></b><span> </span></td>
<td><span>200</span><span> </span></td>
<td><span>HTTP</span><span> </span></td>
<td><span>Tunnel to</span><span> </span></td>
<td><span>orgname</span><span>.crm.dynamics.com:443</span><span> </span></td>
<td><span>0</span><span> </span></td>
<td><span> </span></td>
<td><span> </span></td>
</tr>
<tr>
<td><b><span>16</span></b><span> </span></td>
<td><span>200</span><span> </span></td>
<td><span>HTTPS</span><span> </span></td>
<td><span>orgname</span><span>.crm.dynamics.com</span><span> </span></td>
<td><span>/</span><span>api</span><span>/data/v8.1/contacts?$select=firstname&amp;$filter=contains(firstname,'Peter')</span><span> </span></td>
<td><span>124</span><span> </span></td>
<td><span>no-cache; Expires: -1</span><span> </span></td>
<td><span>application/json; odata.metadata=minimal</span><span> </span></td>
</tr>
</tbody>
</table>
<span> Let's double click <strong>Frame 3#</strong></span>

GET <strong>https://login.windows.net/&lt;ClientID/oauth2/authorize?resource=https%3A%2F%2Forgname.crm.dynamics.com%2F&amp;client_id=&lt;ClientId&gt;&amp;response_type=code&amp;redirect_uri=http%3A%2F%2Fgo%2F</strong>&amp;client-request-id=4e9ee0d9-516a-455a-9f5d-373eea250208&amp;prompt=login&amp;x-client-SKU=.NET&amp;x-client-Ver=2.22.0.0&amp;x-client-CPU=x64&amp;x-client-OS=Microsoft+Windows+NT+6.2.9200.0 HTTP/1.1
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.2; WOW64; Trident/7.0; .NET4.0C; .NET4.0E; Tablet PC 2.0)
Host: login.windows.net
Connection: Keep-Alive

This is the request sent to universal online URL. When we go and see the result in<strong> Fame 14#</strong>, see the response:

HTTP/1.1 200 OK
Cache-Control: no-cache, no-store
Pragma: no-cache
Content-Type: application/json; charset=utf-8
Expires: -1
Server: Microsoft-IIS/8.5
Strict-Transport-Security: max-age=31536000; includeSubDomains
X-Content-Type-Options: nosniff
client-request-id: 4e9ee0d9-516a-455a-9f5d-373eea250208
x-ms-request-id: 4178e998-36e1-4dd7-ab56-8ebd0936904a
P3P: CP="DSP CUR OTPi IND OTRi ONL FIN"
Set-Cookie: esctx=AQABAAAAAADRNYRQ3dhRSrm-4K-adpCJdtApztEfq-GRTdvfueyNhBbkavXHFfnJCcP7lrKEtfnxxYThlX9R_wwMC_36VsfpmQe2a-K8OoAeJfc0EfP1o2SRDXwCPkmzHMDp5pU7XnFBDyupj7pXj2YSavw-coII5LgmagCSKlZCPG_ZBiygejDCDTdzsbtBG0tv1QXNnikgAA; domain=.login.windows.net; path=/; secure; HttpOnly
Set-Cookie: x-ms-gateway-slice=006; path=/; secure; HttpOnly
Set-Cookie: stsservicecookie=ests; path=/; secure; HttpOnly
X-Powered-By: ASP.NET
Date: Sun, 11 Dec 2016 16:12:57 GMT
Content-Length: 3148

{"token_type":"<strong>Bearer</strong>","scope":"user_impersonation","expires_in":"3599","ext_expires_in":"10800","expires_on":"1481476378","not_before":"1481472478","resource":"https://orgname.crm.dynamics.com/","access_token":"<em><strong>&lt;json response.&gt;</strong></em>"}

If there was an error, you would have seen an error message in Json format thrown by Azure. Let's decrypt your Json token to understand what does that mean; Copy the value and go to <a href="http://jwt.calebb.net/">http://jwt.calebb.net/</a>. (<strong>Third-Party Website). </strong>You will see the following content when you decrypt the token:
<code>
{
typ: "JWT",
alg: "RS256",
x5t: "RrQqu9rydBVRWmcocuXUb20HGRM",
kid: "RrQqu9rydBVRWmcocuXUb20HGRM"
}.
{
aud: "https://Orgname.crm.dynamics.com/",
iss: "https://sts.windows.net/&lt;ClientId&gt;/",
iat: 1481472478,
nbf: 1481472478,
exp: 1481476378,
acr: "1",
amr: [
"pwd"
],
<strong><em>appid: "&lt;&lt;ClientId &gt;&gt;",</em></strong>
appidacr: "0",
e_exp: 10800,
family_name: "Ghai",
given_name: "Apurv",
ipaddr: "00.00.00.00",
name: "Apurv Ghai",
oid: "99914ea0-1e03-4850-a963-8f240a87d152",
platf: "14",
puid: "1003BFFD97A8CE14",
scp: "user_impersonation",
sub: "tzNxkMSUdSHCNt29AQg3adxxQTkhE7-wXc6woLesVnY",
tid: "1b69ec75-fc81-4af1-af57-e567d6ed7383",
unique_name: "admin@domain.onmicrosoft.com",
<strong>upn: "admin@domain.onmicrosoft.com",</strong>
ver: "1.0",
wids: [
"62e90394-69f5-4237-9190-012177145e10"
]
}.
[signature]
</code>
This json format confirms the credentials of the logged on user. This way you can really understand what's going on with your application. Further now, this token will be used to make webapi requests in Dynamics 365,
<table>
<tbody>
<tr>
<th>#</th>
<th>Result</th>
<th>Protocol</th>
<th>Host</th>
<th>URL</th>
<th>Body</th>
<th>Caching</th>
<th>Content-Type</th>
<th>Process</th>
<th>Comments</th>
<th>Custom</th>
</tr>
<tr>
<td>16</td>
<td>200</td>
<td>HTTPS</td>
<td>orgname.crm.dynamics.com</td>
<td>/api/data/v8.1/contacts?$select=firstname&amp;$filter=contains(firstname,'Peter')</td>
<td>124</td>
<td>no-cache; Expires: -1</td>
<td>application/json; odata.metadata=minimal</td>
<td>crm_sdk_samples:7448</td>
<td></td>
<td></td>
</tr>
</tbody>
</table>
In <strong>Frame 16#</strong>, you see that we are doing API Call to Dynamics 365. Here's the header format:

GET<strong> https://orgname.crm.dynamics.com/api/data/v8.1/contacts?$select=firstname&amp;$filter=contains(firstname,'Peter') HTTP/1.1</strong>
Authorization: <strong>Bearer &lt;encrypted part removed&gt;</strong>
Accept: application/json
OData-MaxVersion: 4.0
OData-Version: 4.0
Cache-Control: no-cache
Host: orgname.crm.dynamics.com

Let's map this request with the piece of code:
<pre>public async Task SearchExistingRecord(string entityName, string filter)
        {
            <em><strong>httpClient = CreateDynHttpClient(AccessToken, entityName);
            string completedFilterCondition = BaseOrganizationApiUrl + "/api/data/v8.1/" + entityName + filter;</strong></em>
            var response = await httpClient.GetAsync(completedFilterCondition);
            response.EnsureSuccessStatusCode();
            if (response.StatusCode == HttpStatusCode.OK)
            {
                var content = await response.Content.ReadAsStringAsync();
                var objParsedContent = JsonConvert.DeserializeObject(content);

                // Do something with response. Example get content:
                Console.WriteLine(objParsedContent);
                Console.WriteLine("Records Found");
                Console.ReadKey();
                //Dispose the Object :: Best Practice
                httpClient.Dispose();
            }
        }</pre>
if you see the highlighted portion after creating the HttpClient we are passing the access token to be sent to every request going to Dynamics 365. Here's the completed response to this request:
<code>
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: application/json; odata.metadata=minimal
Expires: -1
Server: Microsoft-IIS/8.5
REQ_ID: 38e4f5e9-7fd8-4ac6-822f-9631519f08d5
OData-Version: 4.0
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
Date: Sun, 11 Dec 2016 16:12:58 GMT
Content-Length: 124
Set-Cookie: crmf5cookie=!x8uRQEWsVflwvJeyl31zE1vVQ1hrKpIoEAcC4gh9O0uumcDINf2R/MfwM5l2UsVA7AVQX3hQVwkwNNk=;secure; path=/
Strict-Transport-Security: max-age=31536000; includeSubDomains

{
"@odata.context":"https://orgname.crm.dynamics.com/api/data/v8.1/$metadata#contacts(firstname)","value":[

]
}
</code>
Basically, there was no data found with that criteria specified as first name = Peter.

Hope you've enjoyed reading this.

Happy WebAPI'ing

Cheers,

Apurv :)

&nbsp;
