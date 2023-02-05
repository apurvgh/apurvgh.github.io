---
layout: post-default
title:  "Develop your docs site with docfx"
date:   2020-12-21 20:04:00 -0600
PostDate: 2020-12-21
author: apurvghai
ag_categories: AzureDevOps
lang: Yaml, Powershell
---

**Season Greetings :)**

Do you know about DocFx? No? You can [read more](https://dotnet.github.io/docfx/?_blank). You can do a lot more with DocFx. I am covering a scenario when you want to build websites with your favorite languages like using `Yaml`or `Markdown`. You can then host your site on either GitHub Pages, or choose a hosting provider.

The best part is you do not need to write any code for this :) To get started, you need to meet some prerequisites: 

## Power Tools

 - Powershell
 - DocFx basics
 - Knowledge on Markdown (MD) or Yaml
 - Azure DevOps or GitHub Repos `code` (optional) 
 
## Walkthrough

Now that you are all set with your power tools, let us begin and do some fun stuff.

*   ### Install and setup
    
    I prefer using command line tools. You can find docfx instructions [here](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool). If you do not have chocolatey, you can go [here](https://docs.chocolatey.org/en-us/choco/setup) to find the installation instructions.
    
    Go to your powershell console and run below commands:
    
       `choco install docfx -y cd d:\firstDocFxSite\ docfx init -q`
    

*   ### Change your docfx configuration
    
    After running the commands above, you will see a sample docfx.json generated with a set of folders. Change your JSON config as shown below:
    
     ```json
       {
        "build": {
            "content": [
            {
                "files": [
                "*.{md,yml}",
                "**/*.{md,yml}"
                ]
            }
            ],
            "resource": [
            {
                "files": [
                ".attachments/**",
                "images/**.{ico,png}",
                "*.{ico,png}",
                "scripts/**.{js}",
                "web.config"
                ]
            }
            ],
            "globalMetadata": {
            "_appTitle": "apurvghai.com",
            "_appLogoPath": "images/logo.png",
            "_appFaviconPath": "favicon.ico",
            "_enableSearch": true,
            "_enableNewTab": true,
            "_gitContribute": {
                "repo": "https://github.com/apurvghai/samplerepo",
                "branch": "master"
            },
            "_appFooter": "© apurvghai.com  Built using DocFx"
            },
            "template": [
            "default",
            "templates/material"
            ],
            "postProcessors": [
            "ExtractSearchIndex"
            ],
            "force": true,
            "dest": "docs",
            "globalMetadataFiles": [],
            "fileMetadataFiles": [],
            "markdownEngineName": "markdig",
            "noLangKeyword": false
        }
        }
     ```   
    
    If you want to know more about each option inside this configuration, go [here](https://dotnet.github.io/docfx/tutorial/docfx.exe_user_manual.html).
    
*   ### Arrange your folders
    
    Delete **api, apidoc, articles** folders.
    
    Based on the configuration above your folder structure should look like below:
    
     - firstDocFxSite 
     - images 
     - src 
     - docfx.json 
     - index.md 
     - toc.yml
    
*   ### Add files and content
    
    Add content to your index.md
    
     **Welcome to my first docFx website I love markdown :)**
    
    Navigate to `src` folder and create some md files.
    
    Create a new file `Getting-Started.md` and following content
    
    ```md
    ## Getting Started TODO
    ```
    
    You can create more folders or files as you like. This is how my structure looks like:
    
    - firstDocFxSite 
    - images 
    - src 
        - AnotherFolder 
            - Sub-Article-1.md 
            - Article-1.md 
            - Getting-Started.md 
        - docfx.json 
        - index.md 
        - toc.yml
    
*   ### Create table of contents (TOC)
    
    While this is easy to build, please pay close attention on this step. You should add two different TOCs. First should be in the root and another can be in different folders like `src`.
    
    Add following content into your `root\toc.yml`
     ```yml
        - name: apurvghai.com 
          href: / homepage: /
     ```

    Add following content into your `root\src\toc.yml`
    ```yml
        - name: Intro 
          href: ../index.md 
        - name: Getting Started 
          href: Getting-Started.md 
        - name: Article 1 
          href: Article-1.md 
          items: 
            - name: Sub-Article-1.md 
              href: AnotherFolder/Sub-Article-1.md 
    ```

*   ### Compile your website and serve locally
    
    You can build, compile and preview locally. To do that, you will need to switch to powershell. Run this command `docFx docfx.json --serve --force`
    
    Congratulations You have developed your first docFx website :)
    
    ![your-first-site](/programming/azuredevops/markdown-site.png)
    
*   ### Automatic Deployment using Azure DevOps Pipelines (Optional)
    
    If you are an advanced user and want this website to be automatic deployed. You can follow steps below:
    
    *   Configure branch Policy for your repo
    *   Create your build pipeline using classic editor or yaml.
    
    Yaml example
    
     ```yml
    pool: name: Azure Pipelines 
    steps: 
        - powershell: 
          displayName: 'Compile DocFx' 
            Compile choco install docfx -y docfx .\docfx.json 
            
        
        - task: AzureRmWebAppDeployment@4 
          displayName: 'Azure App Service Deploy: <your website name>' 
          inputs: 
            azureSubscription: 'SUBSCRIPTION-NAME (SUBSCRIPTION-ID)' 
            WebAppName: <yourname webname> 
            packageForLinux: 'docs' 
            ScriptType: 'Inline Script' 
            enableCustomDeployment: true
     ```

    **Note**: Search does not work when hosted on Azure App Service. This requires that you add a web.config to serve .json content.
    
    ```xml
        <configuration> 
            <system.webServer> 
                <staticContent> 
                    <mimeMap fileExtension=".json" mimeType="application/json" /> 
                </staticContent> 
            </system.webServer>       
        </configuration>
    ```
    
*   #### Use Custom Design Templates
    
    If you would like to customize your website with available templates, browse this [doc](https://dotnet.github.io/docfx/templates-and-plugins/templates-dashboard.html) to learn more.
    

### Conclusion

DocFx is a very powerful tool which allows you to build troubleshooting guides, code or wiki documentation etc.., and render them with a sophisticated user interface. I hope you find this article useful. Let me know if you have any feedback.