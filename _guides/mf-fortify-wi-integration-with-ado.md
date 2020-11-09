---
title: Microfocus Fortify Web Inspect Integration in Azure DevOps
layout: default
date: 2020-11-09
---

:warning: WebInspect is installed inside ESDC's network, therefore your pipeline agent should also be inside ESDC's network.

## Prerequisites

1. A WebInspect account
2. A WebInspect project
3. A WebInspect scan template

:warning: All three prerequisites are required. If you are missing one of those, you should contact ESDC IT Security.


## Description

Scans are done outside of the pipeline agent by WebInspect itself. The pipeline agent is mostly used to do calls to the server APIs. 

The pipeline concists of shell commands. The first one allows the authentication with the WebInspect server, if you need credentials or forgot them, you should contact IT Security. The second one asks WebInspect to perfom a scan on your application.


## Configurable Variables

Variable | Description
--------- | -----------
WIE_URL | the webinspect url
USERNAME | your webinspect username
PASSWORD | your webinspect password
APP_URL | your application url
APP_ID | your application id in webinspect
SCAN_TEMPLATE_ID | the id of the scan you want to perform
SENSOR_ID | the id of the sensor used for the scan

## Pipeline example
````

variables:
  WIE_URL: https://mlapesd4290.hrdc-drhc.net
  USERNAME: myUsername
  PASSWORD: mySecurePassword
  APP_URL: http://10.13.227.99:8080/WebGoat
  APP_ID: cdf9406e-1ffa-441c-8729-6735f5ca322b
  SCAN_TEMPLATE_ID: 68350857-af1d-4f8b-afac-3ac535ca84ad
  SENSOR_ID: 255f1e7b-1d09-45d5-b025-10d82e35748a

  
  steps:
  - task: PowerShell@2
    inputs:
      targetType: 'inline'
      script: | 
        try {
            Invoke-RestMethod -Method ''POST'' -Uri ''$(WIEURL)/v1/auth'' -SessionVariable ''Session'' -Body @{username = "$(USERNAME)"; password = "$(PASSWORD)"}
        } catch {
            write-warning "$_"
            exit 1
        }
                
        try {
            Invoke-RestMethod -Uri ''$(WIEURL)/v2/scans'' -Method POST -ContentType ''application/json'' -WebSession $Session -Body ''{"name": "Site: $(APP_URL)", "projectVersion":  { "siteId": "$(APP_ID)"}, "scanTemplateId": "$(SCAN_TEMPLATE_ID)", "overrides": { "startMethod": "url", "startUrls": ["$(APP_URL)"], "priority": 1, "sensorID": "$(SENSOR_ID)"}}''
        } catch {
            write-warning "$_"
            exit 1
        }
````