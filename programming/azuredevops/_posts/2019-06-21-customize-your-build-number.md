---
layout: post-default
title:  "Customize your build number"
date:   2019-06-21 16:15:00 -0600
PostDate: 2019-06-21
author: apurvghai
category: azuredevops
category_title: Azure DevOps
lang: Yaml
---

This article will help you understand how to customize your build number for CD/CI pipeline. There are two ways either by using predefined variables or custom variables. 

#### Predefined variables
----

Here is an official [documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml) on using these variables.

My favorite format is  `Date.Month.Year.RunningNumber`. 

- Edit your pipeline
- Go to Options, add this format in the build nunber format textbox  `$(date:dd)$(date:.MM)$(date:.yyyy)$(rev:.r)`

![Custom Build Number](../../../build-screen.png)

Now if you notice I have used periods (.) before each format, this is to make sure you can output the number as `21.06.2019.X`. You can also use a 4 digit revision number by adding multiple variables `r` which would then look like this `$(rev:.rrrr)`. The advantage in this approach is that, you don't have to worry about the incremental build value. DevOps will take care of incrementing revision for you. This may or may not be ideal technique in your business scenario. For further customization and custom logic, let's look into using custom variables. 

#### Custom Variables
---

To define custom variables, you need to edit your pipeline and go to variables tab. See the image below:

![Custom Variables](../../../custom-variables.png)

Once you have a variable defined, you can either use powershell or yaml to consume those variables. Let's take an example of inline powershell script, where you want to define a build number.

```powershell
# TODO:// Write your logic for generating the build numbers

$CustomNumber = "$Day.$Month.$Year" 
Write-Host "##vso[task.setvariable variable=CustomVariable]$CustomNumber"
```

Here is an example of powershell task added to a pipeline. 


##### Powershell Task (Agent Job)
<br>
![Custom Tasks](../../../powershell-tasks.png)
