# MacOSX 

Quick Start:

1. Create a user, login as the user, set umask 022 and run the below command 

```
```

2. Determine the Resource name for the Token Server and create a Resource Identity Token using your Email Address from HexaEight Mobile App
3. Get an API Key if you dont have one
4. Use the generatemachinecode.sh script to assign the Resource name to the Token Server along with the API Key by scanning the QR Code using HexaEight Mobile App
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

