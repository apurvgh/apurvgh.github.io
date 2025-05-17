---
layout: post-default
title: SSIS 2008 Integration with CRM 2011 on-Premise
date: 2012-6-6 3:22:0 - 0600
PostDate: 2012-6-6
author: apurvghai
category: dynamicscrmsdk
category_title: Dataverse SDK
ag_original_link: https://learn.microsoft.com/en-us/archive/blogs/apurvghai/ssis-2008-integration-with-crm-2011-on-premise
lang: C#
---

<p align="justify">This article deals with integrating SSIS 2008 and CRM 2011. You might have faced a lot of issues doing this part. As we know that SSIS 2008 still doesn&rsquo;t support .NET 4.0 Framework so adding .NET 4 compiled assemblies is not a possible task/solution. What&rsquo;s next? We can do this with backward compatibility using CRM v4 SDK. But with this approach we lose on CRM 2011 newer functions. After doing a lot of research I was finally able to write some code over it using .NET Framework 3.5</p>
<h3>Before we get started, please refer the requirements:</h3>
<ul>
<li>Visual Studio 2010</li>
<li>WCF concepts</li>
<li>Hands on CRM SDK 2011</li>
</ul>
Note: I will not cover the SSIS Project creation, for more details you can see <a href="http://msdn.microsoft.com/en-us/library/ms137823.aspx">http://msdn.microsoft.com/en-us/library/ms137823.aspx</a>.
<h3>Walkthrough</h3>
<ul>
<li>Create an empty solution in VS 2010</li>
<li>Right click, to add new Class Library Project</li>
<li>Choose the Target Framework as .NET 3.5</li>
<li>Name the project as &ldquo;CrmProxy&rdquo;</li>
<li>Create a class called &ldquo;CrmHelper.cs&rdquo;</li>
<li>Add service reference to Organization.svc and name it as &ldquo;Crm&rdquo;</li>
<li>Add the following code:</li>
</ul>

````cs
    namespace CrmProxy 
    { 
        public  class CrmHelper 
        {
            public static IOrganizationService GetCRMService(string ServerHost, string OrgName,  string UserName, string Domain,  string Pwd)
            {
                Uri OrgUri = new Uri(string.Format("{0}/{1}/XRMServices/2011/Organization.svc", ServerHost, OrgName));  
                SymmetricSecurityBindingElement symmetricSecurityBindingElement =  new SymmetricSecurityBindingElement(); symmetricSecurityBindingElement.ProtectionTokenParameters = new SspiSecurityTokenParameters();  
                HttpTransportBindingElement httpTransportBindingElement =  new HttpTransportBindingElement(); 
                httpTransportBindingElement.MaxReceivedMessageSize = 1000000000;  
                CustomBinding customBinding = new CustomBinding(); 
                customBinding.Elements.Add(symmetricSecurityBindingElement);  
                TextMessageEncodingBindingElement textMessageEncodingBindingElement = new  TextMessageEncodingBindingElement(MessageVersion.Soap12WSAddressing10, Encoding.UTF8);  
                customBinding.Elements.Add(textMessageEncodingBindingElement); 
                customBinding.Elements.Add(httpTransportBindingElement);  
                EndpointAddress endpointAddress = new EndpointAddress(OrgUri); 
                OrganizationServiceClient organizationServiceClient =  new OrganizationServiceClient(customBinding, endpointAddress); organizationServiceClient.ClientCredentials.Windows.ClientCredential = new NetworkCredential(UserName, Pwd, Domain);  
                return (IOrganizationService)organizationServiceClient;
            } 
        } 
    }
````
<ul>
<li>Above code is the WCF implementation by creating Service client.</li>
<li>You will need to create extensions class for the same</li>
<li>Add new class &ldquo;Extensions.cs&rdquo; and write the relevant code for it.</li>
<li>Compile the code</li>
<li>You will notice a file in your project called &ldquo;app.config&rdquo;</li>
<li>We would need to add the config details as below</li>
</ul>
````xml
<?xml version="1.0"?>
<configuration>
  <configSections>   </configSections>   <system.serviceModel>
    <bindings>
      <customBinding>
        <binding name="CustomBinding_IOrganizationService">
          <!--    WsdlImporter encountered unrecognized policy assertions in ServiceDescription 'http://schemas.microsoft.com/xrm/2011/Contracts/Services':    -->
          <!--    <wsdl:binding name='CustomBinding_IOrganizationService'>    -->
          <!--    <ms-xrm:AuthenticationPolicy xmlns:ms-xrm="..http://schemas.microsoft.com/xrm/2011/Contracts/Services">..</ms-xrm:AuthenticationPolicy>    -->
          <security defaultAlgorithmSuite="Default" authenticationMode="SspiNegotiated"
           requireDerivedKeys="true" securityHeaderLayout="Strict" includeTimestamp="true"
           keyEntropyMode="CombinedEntropy" messageProtectionOrder="SignBeforeEncryptAndEncryptSignature"
           messageSecurityVersion="WSSecurity11WSTrustFebruary2005WSSecureConversationFebruary2005WSSecurityPolicy11BasicSecurityProfile10"
           requireSecurityContextCancellation="true" requireSignatureConfirmation="false">
            <localClientSettings cacheCookies="true" detectReplays="true"
			 replayCacheSize="900000" maxClockSkew="00:05:00" maxCookieCachingTime="Infinite"
			 replayWindow="00:05:00" sessionKeyRenewalInterval="10:00:00"
			 sessionKeyRolloverInterval="00:05:00" reconnectTransportOnFailure="true"
			 timestampValidityDuration="00:05:00" cookieRenewalThresholdPercentage="60" />
            <localServiceSettings detectReplays="true" issuedCookieLifetime="10:00:00"
             maxStatefulNegotiations="128" replayCacheSize="900000" maxClockSkew="00:05:00"
             negotiationTimeout="00:01:00" replayWindow="00:05:00" inactivityTimeout="00:02:00"
             sessionKeyRenewalInterval="15:00:00" sessionKeyRolloverInterval="00:05:00"
             reconnectTransportOnFailure="true" maxPendingSessions="128"
             maxCachedCookies="1000" timestampValidityDuration="00:05:00" />
            <secureConversationBootstrap />  
          </security>
          <textMessageEncoding maxReadPoolSize="64" maxWritePoolSize="16"
           messageVersion="Default" writeEncoding="utf-8">
            <readerQuotas maxDepth="32" maxStringContentLength="8192" maxArrayLength="16384"
			 maxBytesPerRead="4096" maxNameTableCharCount="16384" />  
          </textMessageEncoding>
          <httpTransport manualAddressing="false" maxBufferPoolSize="524288"
           maxReceivedMessageSize="65536" allowCookies="false" authenticationScheme="Anonymous"
           bypassProxyOnLocal="false" decompressionEnabled="true" hostNameComparisonMode="StrongWildcard"
           keepAliveEnabled="true" maxBufferSize="65536" proxyAuthenticationScheme="Anonymous"
           realm="" transferMode="Buffered" unsafeConnectionNtlmAuthentication="false"
           useDefaultWebProxy="true" />
        </binding>  
      </customBinding>    
    </bindings><client>
      <endpoint address="http://://XRMServices/2011/Organization.svc" binding="customBinding" bindingConfiguration="CustomBinding_IOrganizationService" contract="Crm.IOrganizationService"     name="CustomBinding_IOrganizationService">    
      </endpoint>    
    </client>  
  </system.serviceModel>   <startup>
    <supportedRuntime version="v2.0.50727"/>  
  </startup>
</configuration>
````
<ul>
<li>Now add new project to this solution</li>
<li>This time use, Console Application and Provide name as CrmProxyTester</li>
<li>Add the reference of the CrmProxy.dll</li>
<li>Write the following code:</li>
</ul>

```cs
        using System; 
        using System.Collections.Generic; 
        using System.Linq; 
        using System.Text; 
        using CrmProxy; 
        using CrmProxy.Crm;  
        namespace CrmProxyTester 
        {
            class Program
            {
                IOrganizationService serviceProxy;
                static void Main(string[] args)
                {  
                    Program programClassInstance = new Program();  
                    programClassInstance.StartRun();
                }
                
                public void StartRun()
                { 
                    serviceProxy = CrmHelper.GetCRMService("http://<servername>", "OrganizationName", "UserName", "Domain", "Password"); 
                    Entity account = new Entity(); 
                    account.LogicalName = "account"; 
                    account["name"] = "Apurv Ghai is the account"; 
                    serviceProxy.Create(account);
                }
            } 
        }
```
<ul>
<li>In above code I&rsquo;m connecting to CrmProxy and creating an account in CRM</li>
<li>Now, we&rsquo;re all set.</li>
<li>Build the complete solution and run your command application.</li>
</ul>
<p>This completes the testing the project with .NET 3.5 class library. You can use the same in your SSIS project.</p>
<p>I have attached the code for download.</p>
<p><b>Happy Integration!</b></p>
<p>Cheers, <br /> Apurv</p>
<p>&nbsp;</p>

<p><a href="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Components.PostAttachments/00/10/31/58/19/CrmProxy.zip">CrmProxy.zip</a></p>
