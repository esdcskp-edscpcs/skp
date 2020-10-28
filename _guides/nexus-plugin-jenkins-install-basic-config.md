---
title: Installation and Basic Configuration of Nexus Platform Plugin for Jenkins
layout: default
date: 2020-10-28
---

Nexus IQ Server is a policy engine powered by [precise intelligence](https://guides.sonatype.com/iqserver/technical-guides/sonatype-vuln-data/) on open source components. It provides a number of tools to improve component usage in your software supply chain, allowing you to automate your processes and achieve accelerated speed to delivery while also increasing product quality.

The Nexus IQ Server policy engine powers Nexus Firewall, Lifecycle, and Auditor. With IQ Server, you can:

   - Share component intelligence with development teams, helping them make better decisions and build better software.
   - Implement a fully-customizable policy engine that lets you define which components are acceptable and which are not.
   - Integrate with popular development tools like Maven, Eclipse, IntelliJ, Visual Studio, GitHub, Bamboo, and Jenkins.
   - Provide a full suite of supported [REST APIs](https://help.sonatype.com/iqserver/automating/rest-apis) that provide access to core features for custom implementations.

## Installation

   1. Search Nexus Platform in Manage Plugins in Jenkins.
   2. Download now and install after restart.

## Add cert to Jenkins (Skip if already done)

   1. Download Nexus IQ Server Certificate (Ask SADE for the URL)
   2. On Jenkins server, open powershell as admin
   3. `cd "<jenkins_install_folder>\jre\bin"`
   4. Enter command

      `.\keytool.exe -importcert -alias Sonatype -keystore ..\lib\security\cacerts -file "filepath_of_cert" -trustcacerts`
	
   5. enter password for keystore - default is "changeit"
   6. Trust the certificate
   7. Restart Jenkins

## 3. Connecting Jenkins to IQ Server

a. Go to Manage Jenkins
b. Configure System
c. Scroll to Sonatype Nexus and add Nexus IQ Server
d. Add server URL:

   > Ask SADE for the URL

e. Add credentials to IQ Server
f. Test Connection

## 4. Adding Sonatype task to Jenkins Freestyle Projects

a. In the Build section of your project configuration screen, click the 'Add Build Step' drop-down and select 'Invoke Nexus Policy Evaluation'
b. Fill in the step field values

      i. Stage: Build
      ii. Select an IQ Application (Use Sandbox application for testing purposes)

c. Save
d. Run build.

## 5. Adding Sonatype to Jenkins Pipeline

	a. In the Pipeline section of a Pipeline project configuration screen, open the Snippet Generator by clicking the 'Pipeline Syntax' link
	b. In the Steps section of the Snippet Generator window, select 'nexusPolicyEvaluation: Invoke Nexus Policy Evaluation'
	c. Fill in the step field values

		i. Stage: Build
		ii. Select an IQ Application (Use Sandbox application for testing purposes)

	c. After filling in the step field values, copy the generated snippet into your pipeline script.

	Note: A build of the project must be completed in the workspace before the Nexus Evaluation. Nexus will look for all allowable file extensions and scan them.