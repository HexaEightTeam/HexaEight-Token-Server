
## HexaEight Token Server Authorization

This section contains the authorization files for HexaEight Token Server

HexaEight Server implements two authorzation layers, the first one at the Token Server Level and the second one at the Resource Layer.

These sample authorization files belong to the authorization layer at Token Server Level and should not be confused with th authorization required for Resources

HexaEight Token Servers implements [CASBIN](www.casbin.org) to enforce Authorization at the first layer. The custom access model definition allows Token Server to accept wild card entries to allow accesss for Email Domains to be used during authorization.  

Before you begin this section, you will need to have the below two information 

Token Server Resource Name (Also referred below as tokenserver> : you can run the following command to get the Token Server Resource Name
  
    HexaEight_Token_Issuer --showowner
    
Client ID : You will need to generate a Client ID for your Application using the following Command. Make note of the Client ID once the command has completed running succeessfully.

    HexaEight_Token_Issuer --clientid

---

  Model.conf - Contains the Custom Access Model Definition for Enforcers. 

  Policy.csv - Sample authorization definitions that enforce who is allowed to login and fetch Client tokens

  captchamodel.conf - Contains the Custom Access Model Definition for Enforcers used only for fetching Encrypted Captcha

  captchapolicy.csv - Sample authorization definitions that enforce who is allowed to fetch Encrypted Captcha


Model.conf and captchamodel does not require modifications and can directly be deployed on the token server.

Policy.csv - Examine the sample entires provided and add entries to suit your environment. 
The summary of entries required in the policy file are detailed below. The fourth field in the below samples are CONSTANTS and should NOT be changed. 


1. Which EMail Domain needs are allowed to login and fetch Client Tokens from your Token Server.

If you want to allow a specific domain, add an entry only for that domain, or else if you want to allow any domain use a wild card entry.

  p, /*@mydomain.com, <tokenserver>, <clientid>, login  
  p, /*@mydomain.com, <tokenserver>, <clientid>, ask
  p, /*@mydomain.com, <tokenserver>, <clientid>, poke

            
                    OR
      
  p, /*@*.*, <tokenserver>, <clientid>, login
  p, /*@*.*, <tokenserver>, <clientid>, ask
  p, /*@*.*, <tokenserver>, <clientid>, poke
  
2. Which User Agents or Client Application are allowed to request Client Tokens on behalf of the user. In Authorzation terms, this is referreed to as the what part which can access your token server.  Each Client application is allowed based on a Client Hash Generated during application development.  If its a browser based application, the hostname where the application is hosted is the client hash. 

You can use * in the clienthash if you want to allow any Client, but we advise against doing this esp if you are hosting a browser based application, since anyone may be able to crawl your browser app and rehost at another location and use your token server. We prefer you use a * only during Application Development and switch to valid Client hashes when deploying it in Production.

  p, /*, <tokenserver>, <clientid>, clientaccess
  
                    OR

  p, <clienthash>, <tokenserver>, <clientid>, clientaccess


1. Define the Email users and EMail Domains that are allowed to login, fetch User info and Client Tokens
2. Define the Client Applications using the Client Hash to enforce specific Client Application to access the Token Server
3. Define if EMail users are allowed to contact other Email users and define the allowed permissions
4. Define if EMail users are allowed to access Resource Servers
5. Define the permissions for Resource Servers to fetch Client Tokens of EMail users to respond back to user requests.
6. Add Deny rules for any specific users who should not be issued Client Tokens.
