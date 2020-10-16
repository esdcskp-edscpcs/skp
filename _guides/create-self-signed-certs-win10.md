---
title: How to create self-signed certificates on Windows 10
layout: default
date: 2020-10-16
---

# Background

McAfee scans contents of code and triggers when it gets nervous about certain behaviours (like playing in registry or invoking other programs, etc). It can block in-house programs as well as off the shelf software. The simplest solution is to recognize the source as being trusted. For instance, if Adobe issues a piece of software, McAfee will trust it because we trust their reputation (even if some of the code is suspicious).

The recommended solution is to issue "Code Signing" certificates to programmers. In McAfee Threat Intelligence Exchange (TIE), a trusted reputation could be given to the provenance (Certificate Issuer) and we would automatically trust all the programmers. This is the clean and maintainable solution. If a programmer sold his ESDC certificate on the Dark Web and we found out about, the Certificate could be revoked by the CA or could be assigned a non-trust to that particular certificate in TIE.

## Instructions to create self-signed certificates

1. Open a PowerShell window in Administrator mode, and enter the following command: 

   `New-SelfSignedCertificate -CertStoreLocation Cert:\LocalMachine\My -DnsName "mysite.local" -FriendlyName "MySiteCert" -NotAfter (Get-Date).AddYears(10)`

   This will create a self-signed certificate specific for mysite.local that is valid for 10 years. You can modify the number of years by changing the value in the AddYears function.
   
   ![Create a new certificate using PowerShell](../assets/create-self-signed-certs/instruction-1.PNG)
   
2. 