# HexaEight-Token-Server

HexaEight Token Server issues Client Tokens which are Application specific Asymmetric shared keys to users and resource servers in order to encrypt information for authentication using HexaEight Sessions.  HexaEight Sessions uses a Token-less authentication approach to authenticate and autorize users. 
For more information Visit [HexaEight](www.hexaeight.com)

This Repository contains the links to HexaEight Token Server Executables. The Zip Files also contain HIC files required verify the authenticity of the Executables.

### HexaEight Token Server is currently available for the following Platforms

#### [Windows (64 bit version)](https://www.hexaeight.com/downloads/HexaEight_Token_Issuer/win-x64/HexaEight_Token_Issuer-win-x64.zip) 
  
#### [Linux (64 bit version)](https://www.hexaeight.com/downloads/HexaEight_Token_Issuer/linux-x64/HexaEight_Token_Issuer-linux-x64.zip) 

#### Mac OSX (64 bit version) - Coming Soon

#### [Raspberry Pi (ARM)](https://www.hexaeight.com/downloads/HexaEight_Token_Issuer/linux-arm/HexaEight_Token_Issuer-linux-arm.zip)

In order to run HexaEight Token Server on your machine you need a certficate file issued by HexaEight also known as HexaEight Issued Certificates (HIC)

HexaEight Token Server Executables are NOT digitally signed using Code certificates.  As such they may be prone to modifications.  Hence we have introduced the concept of HexaEight Certificates which are an alternative to Code signing certificates to prevent Executables from being tampered.

**NOTE: DO NOT change the name or rename HexaEight Executables since they protected by HIC, the executable wont run if the name of the executable is changed.**

### Sample output of the executable.

```
c:\tokens>HexaEight_Token_Issuer.exe --help

HexaEight_Token_Issuer 1.6.801
(c) HexaEight.com All Rights Reserved

  -a, --apikey                 Set Rapid API Key

  -q, --quickmode              Enable Quick Captcha Application Layer Encryption Mode for AES Encryption/Decryption

  -s, --setresourceowner       Set Resource Owner

  -g, --generatemachinecode    Generate Machine Code

  -o, --showowner              Displays Current Resource Owner

  -c, --clientid               Register and Generate a New Client ID For an App

  -f, --fetchcert              This is used Internally by HexaEight Team

  -h, --filehash               Show the SHA512 Hash of this file

  -v, --verifycert             Use this to Verify the Authencity of this Software

  -p, --saveconfig             Use this option to push the configuration policies to s3 Cloud Storage

  -r, --getconfig              Use this option to retrieve the configuration policies from s3 Cloud Storage

  --help                       Display this help screen.

  --version                    Display version information.

```
  
  ### Quick Install Instructions:
 
 
Ensure you have generated an EMail Login Token as well as Resource Token using HexaEight Authenticator Mobile Application and you have subscribed to one of our Plans and obtained your [API Key](https://rapidapi.com/hexaeight-hexaeight-default/api/hexaeight-sso-platform/pricing) 
 
 Assuming you want to host applications in myresource.mydomain.com, you will authorize HexaEight Token Server in the domain as shown below
 
--- 

                            Authorizing The Token Server


```
c:\HexaEightTS>HexaEight_Token_Issuer.exe --generatemachinecode
Enter Resource Name
myresource.mydomain.com
Resource Saved Successfully.


Scan the Below QRCode using HexaEight Resource Identity to Authorize This Machine Token


          <QR Code will be displayed>


If you are having Trouble scanning the the above QR Code, alternatively you can paste the below URL in any browser and scan the QR Code usign HexaEight Resource Identity Token To Authorize This Machine Token

https://api.qrserver.com/v1/create-qr-code/?size=400x400&data=Hexa8MTOKN|5CE8BAD8875036CC0D2BC6CDA4174946E58091DFBFEEF7397EF7F567AA4FFD22C4C8C59BA73CE52E55F9AB0A1560CA442BA2E1FF28EEA7F9DA39D3F5A2893B13

Press Any Key Once QR Code Has Been Authorized Using HexaEight Authenticator Mobile App...
Login Token Saved Successfully.

Enter API Key
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
API Key Configuration Saved Successfully.

```
--- 

### Remember to scan the QR Code using the Resource Identity Token of myresource.mydomain.com and authorize it using a valid EMail Login Token belonging to mydomain.com

---

            Generating Client ID For Application:

```
c:\HexaEightTS>HexaEight_Token_Issuer.exe --clientid

Enter Client Application Name : MyResource Sample Application 1.0

Enter the List of Allowed Email Domains seperated by comma [Ex : gmail.com, yahoo.com ]. Enter * if you wish to allow any Email Domain : *

Enter the List of Allowed Client Hashes seperated by comma [Ex : E2985273..., F03E854A.... ] Enter * if you wish to allow any Client  : *


Client ID : 69E17A20950969DCB551BC68542876E7566EE19E

```
--- 

You can now use this Client ID during your Application Development and point it to this token server to enable Authentication and Authorization.

**The token server can only allow specific EMail domains and Clients to login and restrict everyone else, BE CAREFUL when using * in List Of Client Hashes since it can allow an attacker to spoof your application. We will advise using * in List of Client Hashes, only when the application is under Development**



--- 

                Running Token Server

```
c:\HexaEightTS>HexaEight_Token_Issuer.exe

Current Resource Owner:myresource.mydomain.com
----------------------

Note: Ensure to verify the permissions of the current Directory
and remove everyone except Service owner for Security purposes.

HexaEight Tokens
----------------
Default Swagger URL : http://hostname:PORT/swagger/index.html
If you do not have a appsettings.json file the default PORT is 5000
Fetching ASK Keys For auth.hexaeight.com... Done
Determining and Completing List of External Resources For Fetching ASK Keys... Done

HexaEight Token Server Started Successfully

```
--- 


          Enabling HTTPS For Token Server
  
```

c:\HexaEightTS>type appsettings.json
{                                                                               
  "Kestrel": {                                
    "Endpoints": {                                                              
      "Http": {                                
        "Url": "http://0.0.0.0:8080"                                            
      },                                                                        
        "HttpsInlineCertFile": {                                                
        "Url": "https://0.0.0.0:8443",                           
        "Certificate": {                                         
          "Path": "certificate.pfx",             
          "Password": "CertificatePassword"               
        }                                          
      },                                       
    }                                                      
  }                                          
} 

```
--- 
**HexaEight Token Server Does Not Require HTTPs except for fetching Encrypted Captchas for user Email address (sent over the network in plain text)**

If you wish to Enable HTTPs create a file called appsettings.json as shown above in the same HexaEight Token Server Directory with the above contents. 

Restart HexaEight Token Server for the change to take effect.

---
**HexaEight Verified Hash Values**

```
Windows X64 : e19bbc208973858ff01fb7451c0df99665cde12383f247abb843f8775450f30df414a1b6f02a798d2eb22b9f42abe17518976e02f53658ad288a92b854ec7e1d
Linux X64   : b38c2cb8794f18011e7d989f70f0bdc97f6817ffff50022f66f05c5a0462e4fccffbf17dae62bc1488caaaa6a012bd6401324935c4d2ea7fb6205d9a3e2aca20
OSX X64     : 
ARM (Pi)    : 586d5d76032368c264c5d0aa830a474a93b983ef0cc1c6101d4552ae3a6bb33c839a1c8f42d9c149952a87e357a28f2f8c637f9de6900b7303c4e986375c5037
```
---
The above values can be verified by using the --verifycert option. Ensure that the .hic files are located in the same folder as the executable. The below output was captured on a Linux Machine running Ubuntu X64

```
$ ./HexaEight_Token_Issuer  --verifycert
-----------------------------------------------------------
FileName : HexaEight_Token_Issuer
VerifiedHash : b38c2cb8794f18011e7d989f70f0bdc97f6817ffff50022f66f05c5a0462e4fccffbf17dae62bc1488caaaa6a012bd6401324935c4d2ea7fb6205d9a3e2aca20
HashAlgorithm : SHA512
Description : HexaEight_Token_Issuer 1.6.801
Publisher : AUTH.HEXAEIGHT.COM
Issuer : AUTH.HEXAEIGHT.COM
Certificate Issued At : 27875831
Certificate Expiry At : 28135020
AuthenticData : This File or Sofware [HexaEight_Token_Issuer] has been verified successfully.

The Hash can also be manually verified by combining the following properties below: 
FILENAME followed By HASHOFTHISFILE followed by DESCRIPTION
The above SHA512 Hash should match with b38c2cb8794f18011e7d989f70f0bdc97f6817ffff50022f66f05c5a0462e4fccffbf17dae62bc1488caaaa6a012bd6401324935c4d2ea7fb6205d9a3e2aca20

Note: This message wont be displayed if this File or Software was altered or tampered.
This is a Software Publisher Certificate Published by AUTH.HEXAEIGHT.COM and Certified by AUTH.HEXAEIGHT.COM
This message was generated at 1/1/2023 5:11:56 AM
-----------------------------------------------------------
```
