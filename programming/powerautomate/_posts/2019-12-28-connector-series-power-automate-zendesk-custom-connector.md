---
layout: post-default
title:  Connector Series - Power Automate Zendesk Custom Connector
date:   2019-12-28 20:43:00 -0600
PostDate: 2019-12-28
author: apurvghai
category: powerautomate
category_title: Power Automate
lang: 
---

 This article is targeted to users who have hands on experience with power automate. If you have questions about creating connectors, then I suggest you to read Microsoft's [official documentation](https://docs.microsoft.com/en-us/connectors/custom-connectors/define-blank) to familiarize yourself. 

 <div class="tip">Did you know that you can use built-in Power Automate Zendesk connector? <a href="https://docs.microsoft.com/en-us/connectors/zendesk/" target="_blank">Read more..</a></div><br/>

#### Walkthrough

 - **Activate API connection inside Zendesk**

    Navigate to this URL https://YOURURL.zendesk.com/agent/admin/api/settings. You can also read the [official article](https://support.zendesk.com/hc/en-us/articles/115002555167-Using-the-API-dashboard#enabling_password_or_token_access) to know more about enabling tokens. <br />

   <div class="tip">You should find a switch to enable the token access. Create a token and save it in a secure place.</div>

- **Setup Postman**

   I use postman whenever I create custom connectors. This helps with testing the approach and then simplifies the process. Once you've created a collection then you can easily use that to upload it. Let's get on with the steps:

   * Create a get request with Search API Url. Example: https://YOURURL.zendesk.com/api/v2/search.json?query=<YourSearchQuery>
   * Navigate to authorization tab and select **Basic Auth**. You will need to provide your username (registered email `youremail@yourdomian.com/token`) and password (API Token). If you plan to send a base64 encoded token then make sure to send encoded token in this format `youremail@yourdomian.com/token:<< actualtoken >>`.
   * Hit send and voila! You have your results.
    
   <br /><a data-flickr-embed="true" href="https://www.flickr.com/photos/186248049@N06/49290914528/in/album-72157712408017096/" title="postman-api-results"><img src="https://live.staticflickr.com/65535/49290914528_08a74d762c_c.jpg" width="800" height="487" alt="postman-api-results"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>

- **Create a custom connection inside Flow using Postman collection**

  Follow this [link](https://docs.microsoft.com/en-us/connectors/custom-connectors/define-postman-collection) to understand how to create flow connector using Postman. Once you've imported the collection. You will see a similar screen like shown below:
  
  <a data-flickr-embed="true" href="https://www.flickr.com/photos/186248049@N06/49291403746/in/dateposted-public/" title="after-import-connector"><img src="https://live.staticflickr.com/65535/49291403746_8f2535af27_c.jpg" width="800" height="344" alt="after-import-connector"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>

- **Testing your API**

  <a data-flickr-embed="true" href="https://www.flickr.com/photos/186248049@N06/49291403756/in/album-72157712408017096/" title="zendesk-flow-results"><img src="https://live.staticflickr.com/65535/49291403756_4b396a14ca_c.jpg" width="747" height="787" alt="zendesk-flow-results"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>

   I hope you enjoyed reading this.  Please leave your comments below. 

   Happy automating! :)