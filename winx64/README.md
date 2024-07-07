#### [Windows (64 bit version)](https://github.com/HexaEightTeam/HexaEight-Token-Server/releases/download/Production-1.6.8/win-64.zip) 
# Windows x64 


## Download Link : [Windows (64 bit version)](https://github.com/HexaEightTeam/HexaEight-Token-Server/releases/download/Production-1.6.8/win-64.zip) 

Quick Start:

1. Create a normal windows local or domain account, login into the account, run powershell (change to your Home Directory c:\Users\<yourhomedirectory>) and run the below commands


```
Invoke-WebRequest "https://www.hexaeight.com/downloads/HexaEight_Token_Issuer/win-x64/HexaEight_Token_Issuer-win-x64.zip" -UseBasicParsing -Outfile HexaEight_Token_Issuer-win-x64.zip
Expand-Archive .\HexaEight_Token_Issuer-win-x64.zip -DestinationPath .
cd tokenserver

```

2. Determine the Resource name for the Token Server and create a Resource Identity Token using your Email Address from HexaEight Mobile App. Ideally a domain resource name like auth.yourdomain.com would be ideal since this is the name that shows up during user authentication.  To associate your domain name with the token server, you first need to use HexaEight Official Mobile app and use your email id to create a Domain Resource Token. Follow the instructions since you need to add a TXT record in your domain DNS. Once the TXT record is added you can use the Domain Resource Identity token to scan the QR Code which will be displayed when using the generatemachinecode option described in Step 4
3. Get an [API Key](https://rapidapi.com/hexaeight-hexaeight-default/api/hexaeight-sso-platform/pricing) if you dont have one
4. Run the below command from the PS prompt and assign the Resource name to the Token Server along with the API Key by scanning the QR Code using HexaEight Mobile App

```
.\HexaEight_Token_Issuer.exe --generatemachinecode
```

5. Create a Client Application using the command shown below

```
.\HexaEight_Token_Issuer.exe --clientid
```

6. Download [sample Authorization files](https://github.com/HexaEightTeam/HexaEight-Token-Server/tree/main/authorization-samples) and modify the authorization Policies to your Client Application. The PS command to download the policy files is shown below
```
Invoke-WebRequest "https://www.hexaeight.com/downloads/HexaEight_Token_Issuer/policysamples.zip" -UseBasicParsing -Outfile policysamples.zip
Expand-Archive .\policysamples.zip -DestinationPath .
```

7. Create a Windows service using any tool like [nssm](https://nssm.cc/) which can automaitcally start HexaEight Token Server as a Service. If you are using NSSM the sample commands are shown below
8. Ensure to open up the firewall port 5000 to allow your application to connect to the Token Server

```
nssm install <ServiceName> C:\Users\<username>\tokenserver\starttokenserver.bat
nssm set <ServiceName> ObjectName ".\<current Username>" "<your Password>"
nssm set <ServiceName> AppDirectory "C:\Users\<username>\tokenserver"
nssm set <ServiceName> Start SERVICE_DELAYED_AUTO_START
```
**Note: The starttokenserver.bat uses %HOMEDIR% to point to the Token Server, Modify the startup batch script to point to the token server location**

---

**HexaEight Token Server by defaults runs on http port 5000 only, if you need to configure it to run on a different port and use https you will need to create appsettings.json file with the below contents after modifying the configuration accodring to your setup**

```{                                                                               
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

Start the Token Server Service and you should be ready to create your first Application by pointing to the Token Server


### Verification Details
```
-----------------------------------------------------------
FileName : HexaEight_Token_Issuer.exe
VerifiedHash : b1acd6fa501fe22b52387d9d6b336146c852b11054e9966cade4324050e5cd0699294a1f15439656e2fb2ba03ab6b4da2bb9eed9b590a4b4c47c2f5e2ca96e4a
HashAlgorithm : SHA512
Description : HexaEight_Token_Issuer 1.6.801
Publisher : AUTH.HEXAEIGHT.COM
Issuer : AUTH.HEXAEIGHT.COM
Certificate Issued At : 28672100
Certificate Expiry At : 28931295
AuthenticData : This File or Sofware [HexaEight_Token_Issuer.exe] has been verified successfully.

The Hash can also be manually verified by combining the following properties below:
FILENAME followed By HASHOFTHISFILE followed by DESCRIPTION
The above SHA512 Hash should match with b1acd6fa501fe22b52387d9d6b336146c852b11054e9966cade4324050e5cd0699294a1f15439656e2fb2ba03ab6b4da2bb9eed9b590a4b4c47c2f5e2ca96e4a

Note: This message wont be displayed if this File or Software was altered or tampered.
This is a Software Publisher Certificate Published by AUTH.HEXAEIGHT.COM and Certified by AUTH.HEXAEIGHT.COM
This message was generated at 07/07/2024 10:31:17
-----------------------------------------------------------
```
