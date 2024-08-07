# [macOSX](https://github.com/HexaEightTeam/HexaEight-Token-Server/releases/download/Production-1.6.8/mac.zip)

## Download Link : [macOSX (64 bit version)](https://github.com/HexaEightTeam/HexaEight-Token-Server/releases/download/Production-1.6.8/mac.zip) 

Quick Start:

1. Create a user, login as the user, set the umask  and run the below commands

```
umask 077
curl "https://github.com/HexaEightTeam/HexaEight-Token-Server/releases/download/Production-1.6.8/mac.zip" -O -J -L
unzip ./HexaEight_Token_Issuer-mac-OSX.zip
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
8. Create a plist file and use launchctl to starts token server post boot phase. A sample File (untested) for startup is provided for reference.

```
sudo launchctl load /Library/LaunchDaemons/com.hexaeight.startup.plist
```


**Ensure to lauch the Token Server as the user and not as ROOT**

**The restarttokenserver.sh can be used to restart the Token Server if the Authorization policies are updated**
**The cleanup.sh should also be scheduled to run the cleanup routine that can detect anomalies in the downloaded Tokens and remove them automatically.**

Sample Output of HexaEight Token Server On Mac Mini M1

```
m1@e16ce1a5-b08b-4614-9681-5eb6d14e8474 tokenserver % uname -a
Darwin e16ce1a5-b08b-4614-9681-5eb6d14e8474 21.1.0 Darwin Kernel Version 21.1.0: Wed Oct 13 17:33:24 PDT 2021; root:xnu-8019.41.5~1/RELEASE_ARM64_T8101 arm64
m1@e16ce1a5-b08b-4614-9681-5eb6d14e8474 tokenserver % ./HexaEight_Token_Issuer --help
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

  -t, --timecode               Synchronize This Token Server using a Time Code

  -v, --verifycert             Use this to Verify the Authencity of this Software

  -p, --saveconfig             Use this option to push the configuration policies to s3 Cloud Storage

  -r, --getconfig              Use this option to retrieve the configuration policies from s3 Cloud Storage

  -m, --samplepolicies         Generate Sample configuration Policy Files

  --help                       Display this help screen.

  --version                    Display version information.


m1@e16ce1a5-b08b-4614-9681-5eb6d14e8474 tokenserver % ./HexaEight_Token_Issuer --verifycert
-----------------------------------------------------------
FileName : HexaEight_Token_Issuer
VerifiedHash : 5d295d556453ec1587d92781d1ccbfe8cecd24071cfbf18fc400ec4863cf685f03f77ea81eda5570f68ebd19705e796991a37b3244e4497484384c89901c14ba
HashAlgorithm : SHA512
Description : HexaEight_Token_Issuer 1.6.801
Publisher : AUTH.HEXAEIGHT.COM
Issuer : AUTH.HEXAEIGHT.COM
Certificate Issued At : 27994977
Certificate Expiry At : 28254165
AuthenticData : This File or Sofware [HexaEight_Token_Issuer] has been verified successfully.

The Hash can also be manually verified by combining the following properties below:
FILENAME followed By HASHOFTHISFILE followed by DESCRIPTION
The above SHA512 Hash should match with 5d295d556453ec1587d92781d1ccbfe8cecd24071cfbf18fc400ec4863cf685f03f77ea81eda5570f68ebd19705e796991a37b3244e4497484384c89901c14ba

Note: This message wont be displayed if this File or Software was altered or tampered.
This is a Software Publisher Certificate Published by AUTH.HEXAEIGHT.COM and Certified by AUTH.HEXAEIGHT.COM
This message was generated at 03/25/2023 07:00:48
-----------------------------------------------------------

```



