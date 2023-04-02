# ARM x64 - Raspberry pi

## Download Link : [Linux (64 bit version)](https://www.hexaeight.com/downloads/HexaEight_Token_Issuer/linux-arm/HexaEight_Token_Issuer-linux-arm.zip) 

Quick Start:

1. Create a user, login as the user, set the umask  and run the below commands

```
umask 077
curl "https://www.hexaeight.com/downloads/HexaEight_Token_Issuer/linux-arm/HexaEight_Token_Issuer-linux-arm.zip" -O -J -L
unzip ./HexaEight_Token_Issuer-linux-arm.zip
chmod 700 tokenserver/HexaEight_Token_Issuer

```

2. Determine the Resource name for the Token Server and create a Resource Identity Token using your Email Address from HexaEight Mobile App. Ideally a domain resource name like auth.yourdomain.com would be ideal since this is the name that shows up during user authentication.  To associate your domain name with the token server, you first need to use HexaEight Official Mobile app and use your email id to create a Domain Resource Token. Follow the instructions since you need to add a TXT record in your domain DNS. Once the TXT record is added you can use the Domain Resource Identity token to scan the QR Code which will be displayed when using the generatemachinecode option described in Step 4
3. Get an [API Key](https://rapidapi.com/hexaeight-hexaeight-default/api/hexaeight-sso-platform/pricing) if you dont have one
4. Run generatemachinecode.sh script and assign the Resource name to the Token Server along with the API Key by scanning the QR Code using HexaEight Mobile App
5. Create a Client Application using the command shown below

```
$HOME/tokenserver/HexaEight_Token_Issuer --clientid
```
6. Enable authorization for the Client Application using the [sample Authorization files](https://github.com/HexaEightTeam/HexaEight-Token-Server/tree/main/authorization-samples)
7. Start the Server using start_tokenserver.sh script and verify tokenserver.log and tokenserver.err for errors
8. Add entries to /etc/rc.local to open up firewall ports as well as start the Token Server under user account as well as run the cache clean up periodically

```
firewall-cmd --zone=public --add-port=httpport/tcp
firewall-cmd --zone=public --add-port=httpsport/tcp
su - hexa8server -c '$HOME/tokenserver/start_tokenserver.sh &'
su - hexa8server -c '$HOME/tokenserver/cleanup.sh &'

```

**Ensure to lauch the Token Server as the user and not as ROOT**

**The restarttokenserver.sh can be used to restart the Token Server if the Authorization policies are updated**

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


