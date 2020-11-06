---
title: Microfocus Fortify SCA Integration in Azure DevOps
layout: default
date: 2020-11-06
---

## Prerequisites

1. A Software Security Center authnetification token
2. A Software Security Center project

:warning: All three preriquisites are needed. If you are missing one you should contact ESDC IT Security


## Description
Scans are done inside the agent and uploaded to Software Security Center server. Once your application is built, SCA will be installed on your agent and the scan will be performed afterward.

Pipeline steps:
1. Build your application
2. Install SCA on the agent
3. Performe the scan and upload result to Software Security Center

## Install SCA on the agent

Steps:
1. Contact IT Security to obtain SCA archive file
2. Upload this file to your project Artifacts
3. In the azure pipeline download the artifacts

An example of step 3 is available in the example section

## Performe the scan

Steps:
1. Clean: Removing SCA temporary files
2. Translate: Translating the source code into an intermediate translated format
3. Scan: Scanning the translated code and producing security vulnerability reports
4. Upload: Uploading the results to software security center

:warning: How you do the scan depends on the programming language of the application. For more information about these steps you can go to the official [SCA documentation](https://www.microfocus.com/documentation/fortify-static-code-analyzer-and-tools/1810/SCA_Guide_18.10.pdf) 



## Configurable Variables

Variable | Description
--------- | -----------
SSC_URL | the software security center url
SSC_TOKEN | your software security center token
APP_NAME | your application in software security center
APP_VERSION | your application version in software security center
SCA_BIN | the path to SCA binary

## Pipeline Examples

Installing SCA on the agent from Artifacts

:warning: This presuppose that you have SCA, tar achive, as an artifact available in your Azure Devops project. 

````
- stage : SCA
    displayName: SCA
    jobs:
      - job:
        steps:
          - download: current
            artifact: build
          
          - task: UniversalPackages@0
            displayName: Download SCA artifact  
            inputs:
              command: 'download'
              downloadDirectory: '$(System.DefaultWorkingDirectory)'
              feedsToUse: 'internal'
              vstsFeed: 'b1be6f55-20c0-4034-9a7c-14009996c54b/fe527817-8cb3-4f8b-8eb1-a5349267dc9b'
              vstsFeedPackage: '14f60f64-658d-4fd6-ad0a-71d6170ef842'
              vstsPackageVersion: '19.2.0'

          - task: UniversalPackages@0
            displayName: Download Fortify license
            inputs:
              command: 'download'
              downloadDirectory: '$(System.DefaultWorkingDirectory)'
              feedsToUse: 'internal'
              vstsFeed: 'b1be6f55-20c0-4034-9a7c-14009996c54b/fe527817-8cb3-4f8b-8eb1-a5349267dc9b'
              vstsFeedPackage: 'bcd5d3ae-4e0a-4e7c-88c7-5a56326a4e65'
              vstsPackageVersion: '1.0.0'

          - task: ExtractFiles@1
            displayName: Extract Tar file
            inputs:
              archiveFilePatterns: '*.tar.gz'
              destinationFolder: '$(System.DefaultWorkingDirectory)'
              cleanDestinationFolder: false
              
          - task: InstallFortifySCA@5
            displayName: Install Fortify SCA on agent  
            inputs:
              InstallerPath: '$(System.DefaultWorkingDirectory)/$(FORTIFY_SCA)'
              VS2013: false
              VS2015: false
              LicenseFile: '$(System.DefaultWorkingDirectory)/$(FORTIFY_LICENSE)'
              RunFortifyRulepackUpdate: true
````

Scanning a java application
````
- task: Bash@3
displayName: Install Maven Plugin
inputs:
    targetType: 'inline'
    script: |
    cd $HOME/HPE_Fortify/HPE_Fortify_SCA_and_Apps/plugins/maven                
    mkdir maven-plugin-bin
    tar -xsf maven-plugin-bin.tar.gz --directory maven-plugin-bin
    cd maven-plugin-bin
    chmod +x install.sh
    ./install.sh

- task: Bash@3
displayName: Clean
inputs:
    targetType: 'inline'
    script: |
    export set PATH=$(SCA_BIN)
    sourceanalyzer -b $(APP_NAME) -clean

- task: Bash@3
displayName: Translate
inputs:
    targetType: 'inline'
    script: |
    export set PATH=$(SCA_BIN)
    sourceanalyzer -b $(APP_NAME) mvn -f $(Pipeline.Workspace)/build clean package

- task: Bash@3
displayName: Scan
inputs:
    targetType: 'inline'
    script: |
    export set PATH=$(scapath)
    sourceanalyzer -b $(APP_NAME) -scan -f $(APP_NAME).fpr

- task: Bash@3
displayName: Upload
inputs:
    targetType: 'inline'
    script: |
    export set PATH=$(scapath)
    fortifyclient -url $(SSC_URL) -authtoken $(SSC_TOKEN) uploadFPR -file $(APP_NAME).fpr -applicationVersionID $(APP_VERSION)
````