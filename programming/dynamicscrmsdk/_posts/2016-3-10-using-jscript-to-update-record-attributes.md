---
layout: post-default
title: WebAPI Series 1 - Using Jscript to update record attributes
date: 2016-3-10 0:44:17 - 0600
PostDate: 2016-3-10
author: apurvghai
category: dynamicscrmsdk
category_title: Dataverse SDK
ag_original_link: https://learn.microsoft.com/en-us/archive/blogs/apurvghai/using-jscript-to-update-record-attributes
lang: C#
---
Hey Folks,

Since the launch of CRM 2016 we have been officially using WebAPI which is something the coolest. I'm not going to talk much here about this but have developed a sample for quick use.

The sample is used for updating an account record.

&nbsp;

function Test() {
var clientUrl = Xrm.Page.context.getClientUrl();
var req = new XMLHttpRequest()
req.open("<em><strong>PATCH</strong></em>", encodeURI(clientUrl + "/api/data/v8.0/accounts(c9270348-57e6-e511-80cd-00155d4b0e09)"), true);
req.setRequestHeader("Accept", "application/json");
req.setRequestHeader("Content-Type", "application/json; charset=utf-8");
req.setRequestHeader("OData-MaxVersion", "4.0");
req.setRequestHeader("OData-Version", "4.0");
req.onreadystatechange = function () {
if (this.readyState == 4 /* complete */) {
req.onreadystatechange = null;
if (this.status == 204) {
//var accountUri = this.getResponseHeader("OData-EntityId");
Xrm.Utility.alertDialog("Done Deal")
}
else {
debugger;
var error = JSON.parse(this.response).error;
Xrm.Utility.alertDialog("An Error Occurred");
}
}
};
req.send(JSON.stringify({ name: "Sample account", telephone1: "011019" }));

}

&nbsp;

I do really want to call out the highlighted word here, "Patch" this is how CRM understands an update happening through WebAPI. I hope this sample helps you write more cool stuff.

<a target="_blank" href="https://github.com/apurvgh/Dynamics365Samples/blob/master/new_webapi.lib.js">Full Code Sample</a>

Happy WebAP'ing :)

&nbsp;

Thanks,
Apurv
