---
layout: post-default
title:  "Versioning your assemblies through build pipelines"
date:   2019-06-21 17:15:00 -0600
PostDate: 2019-06-21
author: apurvghai
category: azuredevops
category_title: Azure DevOps
lang: Yaml, Powershell
---

I love the idea of how we can simply match the build numbers and use them as a verison on the generated assembly. To do this, we can just write a short powershell script inside the pipeline. Please make sure to read my previous article on how to use custom variables [here](/programming/azuredevops/2019/06/21/customize-your-build-number.html). It illustrates adding powershell tasks and using custom variables. 


Once you are done with setting up your custom variables and powershell task. Then, our next step is to go and customize the .net based tasks arguments. We need to add a switch to the build and/or publish tasks depedening on .NET version you are using. Let's the see the steps below:

I'm using this variable `$CustomBuildNumber`. 

##### .NET Core
---

 Go to build task and then to "Arguments" textbox

 Add the switch called "version" 

   ```yaml
    --configuration $(BuildConfiguration) -p:Version=$(CustomBuildNumber)
   ```

  Go to publish task and repeat the same

  ```yaml
   --configuration $(BuildConfiguration) -p:Version=$(CustomBuildNumber) --output $(build.artifactstagingdirectory)
  ```

<p class="tip">It is mandatory to add version to both build and publish for dotnet core</p>

##### .NET Framework 
---

  Go to build task and then to "MS Build Arguments" textbox

  Add the switch called "version" 

```yaml
/p:DeployOnBuild=true /p:WebPublishMethod=Package /p:PackageAsSingleFile=true /p:SkipInvalidConfigurations=true /p:DesktopBuildPackageLocation="$(build.artifactstagingdirectory)\WebApp.zip" /p:DeployIisAppPath="Default Web Site" /p:Version=$(CustomBuildNumber)
```

*This is an example based on App Service pipeline*

<p class="tip">When you add predefined pipeline for .net apps, "Publish symbols path" is added automatically. This contains version number field. We must set the field with the custom variable</p>

![version](../../../version.png)
