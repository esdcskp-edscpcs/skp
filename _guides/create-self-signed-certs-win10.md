---
title: How to create self-signed certificates on Windows 10
layout: default
date: 2020-10-16
---

McAfee scans contents of code and triggers when it gets nervous about certain behaviours (like playing in registry or invoking other programs, etc). It can block in-house programs as well as off the shelf software. The simplest solution is to recognize the source as being trusted. For instance, if Adobe issues a piece of software, McAfee will trust it because we trust their reputation (even if some of the code is suspicious).

The recommended solution is to issue "Code Signing" certificates to programmers. In McAfee Threat Intelligence Exchange (TIE), a trusted reputation could be given to the provenance (Certificate Issuer) and we would automatically trust all the programmers. This is the clean and maintainable solution. If a programmer sold his ESDC certificate on the Dark Web and we found out about, the Certificate could be revoked by the CA or could be assigned a non-trust to that particular certificate in TIE.

## Instructions to create self-signed certificates

### Web Applications

1. Open a PowerShell window as an **Administrator**, and enter the following command: 

   `New-SelfSignedCertificate -CertStoreLocation Cert:\LocalMachine\My -DnsName "mysite.local" -FriendlyName "MySiteCert" -NotAfter (Get-Date).AddYears(10)`

   This will create a self-signed certificate specific for mysite.local that is valid for 10 years. You can modify the number of years by changing the value in the AddYears function.
   
   ![Create a new certificate using PowerShell](../assets/create-self-signed-certs/webapp-create-cert-powershell.PNG)
   
2. Once the certificate is created, you should copy it to the Trusted Root Certification Authorities store. 

3. Using Cortana search in Windows 10, type "certlm.msc" and **Run it as an Administrator**. 

4. In the left panel, navigate to Certificates - Local Computer → Personal → Certificates 
   
   ![Display Certificates Store](../assets/create-self-signed-certs/webapp-display-cert-store.PNG)
	   
5. Locate the created certificate (in this example look under the Issued To column "mysite.local", or under the Friendly Name column "MySiteCert").
   
6. In the left panel, open (but don't navigate to) Certificates - Local Computer → Trusted Root Certification Authorities → Certificates 
   
7. With the right mouse button, drag and drop the certificate from **Personal → Certificates** to **Trusted Root Certification Authorities  → Certificates** opened in the previous step.
   
8. Select "Copy Here" in the popup menu.

### Desktop Applications

1. Open a PowerShell window as an **Administrator**, and enter the following command (Change the CN, FriendlyName and CertStoreLocation parameters accordingly): 

   `New-SelfSignedCertificate -Type Custom -Subject "CN=ESDC Software, O=ESDC, C=CA" -KeyUsage DigitalSignature -FriendlyName "ESDC Test Certificate" -CertStoreLocation "Cert:\CurrentUser\My" -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.3", "2.5.29.19={text}")`

   Note the following details about some of the parameters: 

   - **KeyUsage**: This parameter defines what the certificate may be used for. For a self-signing certificate, this parameter should be set to **DigitalSignature**.
   - **TextExtension**: This parameter includes settings for the following extensions: 
      - Extended Key Usage (EKU): This extension indicates additional purposes for which the certified public key may be used. For a self-signing certificate, this parameter should include the extension string **"2.5.29.37={text}1.3.6.1.5.5.7.3.3"**, which indicates that the certificate is to be used for code signing. 
	  - Basic Constraints: This extension indicates whether or not the certificate is a Certificate Authority (CA). For a self-signing certificate, this parameter should include the extension string **"2.5.29.19={text}"**, which indicates that the certificate is an end entity (not a CA). 

   After running this command, the certificate will be added to the local certificate store, as specified in the "-CertStoreLocation" parameter. The result of the command will also produce the certificate's thumbprint.

   ![Show certificate thumbprint](../assets/create-self-signed-certs/desktop-show-certificate-thumbprint.PNG)
   
2. You can view your certificate in a PowerShell window by using the following commands: 

   `Set-Location Cert:\CurrentUser\My`
   `Get-ChildItem | Format-Table Subject, FriendlyName, Thumbprint`
   
   This will display all the certificates in your local store.
   
   ![Show Local Store](../assets/create-self-signed-certs/desktop-show-local-store.PNG)
   
## Export a certificate (for Desktop applications)

To export the certificate in the local store to a Personal Information Exchange (PFX) file, use the Export-PfxCertificate cmdlet. 

When using **Export-PfxCertificate**, you must either create and use a password or use the "-ProtectTo" parameter to specify which users or groups can access the file without a password. Note that an error will be displayed if you don't use either the "-Password" or "-ProtectTo" parameter.

### Password Usage

   Update **<Your Password>**, FilePath and **<Thumbprint>** accordingly.
   
   `$password = ConvertTo-SecureString -String **Your Password** -Force -AsPlainText`
   `Get-ChildItem –Path cert:\CurrentUser\my\**9312C9A6C9EC840697B2B3FA7E6F7C3A23085F10** | Export-PfxCertificate -FilePath **C:\ESDC-test.pfx** -Password $password`
   
   ![Export Certificate](../assets/create-self-signed-certs/desktop-export-certificate.PNG)
   
   After you create and export your certificate, you're ready to sign your app package with **SignTool**. For the next step in the manual packaging process, see [Sign an app package using SignTool](https://docs.microsoft.com/en-us/windows/msix/package/sign-app-package-using-signtool). 
   
## Security Considerations

By adding a certificate to [local machine certificate stores](https://docs.microsoft.com/en-us/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores), you affect the certificate trust of all users on the computer. It is recommended that you remove those certificates when they are no longer necessary to prevent them from being used to compromise system trust. 
   