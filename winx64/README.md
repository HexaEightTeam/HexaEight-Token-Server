#### [Windows (64 bit version)](https://www.hexaeight.com/downloads/HexaEight_Token_Issuer/win-x64/HexaEight_Token_Issuer-win-x64.zip) 
# Windows x64 

## Download Link : [Linux (64 bit version)](https://www.hexaeight.com/downloads/HexaEight_Token_Issuer/win-x64/HexaEight_Token_Issuer-win-x64.zip) 

Quick Start:

1. Create a normal windows local or domain account, login into the account, run powershell (change to your Home Directory c:\Users\<yourhomedirectory>) and run the below commands


```
Invoke-WebRequest "https://www.hexaeight.com/downloads/HexaEight_Token_Issuer/win-x64/HexaEight_Token_Issuer-win-x64.zip" -UseBasicParsing -Outfile HexaEight_Token_Issuer-win-x64.zip
Expand-Archive .\HexaEight_Token_Issuer-win-x64.zip -DestinationPath .
cd tokenserver

```

2. Determine the Resource name for the Token Server and create a Resource Identity Token using your Email Address from HexaEight Mobile App
3. Get an [API Key](https://rapidapi.com/hexaeight-hexaeight-default/api/hexaeight-sso-platform/pricing) if you dont have one
4. Run the below command from the PS prompt and assign the Resource name to the Token Server along with the API Key by scanning the QR Code using HexaEight Mobile App

```
.\HexaEight_Token_Issuer.exe --showowner
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
