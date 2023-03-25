# MacOSX 

Quick Start:

1. Create a user, login as the user, set umask 022 and run the below command 

```
```

2. Determine the Resource name for the Token Server and create a Resource Identity Token using your Email Address from HexaEight Mobile App
3. Get an API Key if you dont have one
4. Run generatemachinecode.sh script and assign the Resource name to the Token Server along with the API Key by scanning the QR Code using HexaEight Mobile App
5. Create a Client Application using the command shown below

```
$HOME/tokenserver/HexaEight_Token_Issuer --clientid
```
6. Enable authorization for the Client Application using the sample Authorization files
7. Start the Server using start_tokenserver.sh script and verify tokenserver.log and tokenserver.err for errors
8. Change permissions for all the files to 

Ensure to lauch the Token Server as the user and not as ROOT


A Sample Startup file (not tested) is available for reference to start HexaEight Token Server on Bootup using 

```
sudo launchctl load /Library/LaunchDaemons/com.hexaeight.startup.plist
```

Sample Output of HexaEight Token Server Running On Mac Mini M1

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



