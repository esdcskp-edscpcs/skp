---
title: Microfocus Fortify SCA & WI Integration in Jenkins
layout: default
date: 2020-03-09
---

>  This installation guide assume that you already have your development tools installed on the agent 

# Agent Configuration

## Install Fortify SCA

1. Installation guideline can be found in the "ESDC Fortify SCAN 18.20 Install Guide" document
2. The agent needs to be restarted

## Install Fortify Maven plugin (only for Java application)

1. Copy C:\Program Files\Fortify\Fortify_SCA_and_Apps_*version*\plugins\maven\maven-plugin-src.zip to another location
2. Extract the compressed file
3. Inside the extracted folder use the following command : `mvn clean install`
4. Now copy C:\Users\\[username]\\.m2 to C:\Windows\System32\config\systemprofile\\.m2 

## Add WIE self signed certificate

1. With Internet Explorer go to https://mlapesd4290.hrdc-drhc.net/WIE/WebConsole/Applications.aspx
2. Under **More information** click on **Go on to the webpage (not recommended)**
3. Use the login credential provided by IT Security
4. In the **URL bar** click on the **padlock icon** and click on **View Certificate**. From there click on **Install Certificate…**. Choose **Local machine** and click **Next**. Select **Place all certificates in the following store**, click on **Browse** and select **Trusted Root Certification Authorities** and click **Next**. Click **Finish**

# Master Configuration

## Install the Fortify plugin

1. From Jenkins select **Manage Jenkins** -> **Manage Plugins** and click the **Available** tab and type **Fortify**. Then click **Download and install after restart**
2. From Jenkins select **Manage Jenkins** -> **Configure System**. From there scroll down to **Fortify Assessment**. Then enter the following information :
    1. SSC URL : https://mlapesd4289.hrdc-drhc.net:8443/ssc
    2. Authentication token : The token provided by IT Security
    3.Click **Advanced settings** and then click **Test Connection**

## Agent Environment Variables

1. From Jenkins select **Manage Jenkins** -> **Manage Nodes and Clouds** -> **[Your agent]** - > **Configure**. From there scroll down to **Environment variables** and create the following one :
    1. Name : FORTIFY_HOME
    2. Value : [The location of SCA in the agent]

## Configure Credential

1. From your job, click on Configure
2. Next, scroll down to Pipeline Model Definition. You'll see Registry credentials, click on Add and click on your job's name
3. Enter the following information :
    1. Kind : Username with password
    2. Username : The username provided by IT Security
    3. Password : The password provided by IT Security
    4. ID :  A id (this ID will be used in the pipeline script)
4. Click on **Add** 

# Pipeline examples

## SCA pipeline example - Maven

```
node('ITSEC_SCA_01') {
    stage ('clean') {
        fortifyClean addJVMOptions: '', buildID: 'WebGoat-Lecacy', logFile: '', maxHeap: ''
    }
    
    stage ('Update') {
        fortifyUpdate updateServerURL: 'https://update.fortify.com'
    }
    
    stage ('Translate') {
        fortifyTranslate addJVMOptions: '', buildID: 'WebGoat-Legacy', excludeList: '', logFile: '', maxHeap: '', projectScanType: fortifyMaven3(mavenOptions: '"-f" "C:\\WebGoat-Legacy\\pom.xml"')
        
    }

    stage ('Scan') {
        fortifyScan addJVMOptions: '', addOptions: '', buildID: 'WebGoat-Legacy', customRulepacks: '', logFile: '', maxHeap: '14000', resultsFile: 'wg.fpr'
    }
    
    stage ('Upload') {
        fortifyUpload appName: 'TestingSCA', appVersion: '1', failureCriteria: '', filterSet: '', pollingInterval: '', resultsFile: 'wg.fpr'
    }
}
```

## SCA pipeline example - dotNet

```
node('ITSEC_SCA_01') {
    stage ('clean') {
        fortifyClean addJVMOptions: '', buildID: 'SimpleWebApp', logFile: '', maxHeap: ''
    }
    
    stage ('Update') {
        fortifyUpdate updateServerURL: 'https://update.fortify.com'
    }
    
    stage ('Translate') {
        fortifyTranslate addJVMOptions: '', buildID: 'SimpleWebApp', excludeList: '', logFile: '', maxHeap: '', projectScanType: fortifyDotnetSrc(dotnetAddOptions: '', dotnetFrameworkVersion: '3.1', dotnetLibdirs: '', dotnetSrcFiles: 'C:\\SimpleWebApp')
        
    }

    stage ('Scan') {
        fortifyScan addJVMOptions: '', addOptions: '', buildID: 'SimpleWebApp', customRulepacks: '', logFile: '', maxHeap: '14000', resultsFile: 'FortifySimpleWebApp.fpr'
    }
    
    stage ('Upload') {
        fortifyUpload appName: 'TestingSCA', appVersion: '1', failureCriteria: '', filterSet: '', pollingInterval: '', resultsFile: 'FortifySimpleWebApp.fpr'
    }
}
```

## WIE pipeline example

```
node ('ITSEC_SCA_01') {
    withCredentials([usernamePassword(credentialsId: 'wieapi', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
        stage ('WIE')  {
            powershell '''
                try {
                    Invoke-RestMethod -Method 'POST' -Uri 'https://mlapesd4290.hrdc-drhc.net/WIE/REST/api/v1/auth' -SessionVariable 'Session' -Body @{username = $ENV:USER; password = $ENV:PASS}
                } catch {
                    write-warning "$_"
                    exit 1
                }
                
                try {
                    Invoke-RestMethod -Uri 'https://mlapesd4290.hrdc-drhc.net/WIE/REST/api/v2/scans' -Method POST -ContentType 'application/json' -WebSession $Session -Body '{"name": "Site: http://10.13.227.99:8080/WebGoat", "projectVersion":  { "siteId": "cdf9406e-1ffa-441c-8729-6735f5ca322b"}, "scanTemplateId": "68350857-af1d-4f8b-afac-3ac535ca84ad", "overrides": { "startMethod": "url", "startUrls": ["http://10.13.227.99:8080/WebGoat"], "priority": 1, "sensorID": "255f1e7b-1d09-45d5-b025-10d82e35748a"}}'
                } catch {
                    write-warning "$_"
                    exit 1
                }
                '''        
        }
    }
}
```

