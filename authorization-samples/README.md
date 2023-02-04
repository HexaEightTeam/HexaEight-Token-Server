
## HexaEight Token Server Authorization

This section contains the authorization files for HexaEight Token Server

#### [Download Sample Policy Zip](https://www.hexaeight.com/downloads/HexaEight_Token_Issuer/policysamples.zip) 




HexaEight Server implements two authorzation layers, the first one at the Token Server Level and the second one at the Resource Layer.

These sample authorization files belong to the authorization layer at Token Server Level and should not be confused with the authorization required for Resources

HexaEight Token Servers implements [CASBIN](www.casbin.org) to enforce Authorization at the first layer (at the Token Server). This custom access model definition allows the Token Server to accept wild card entries for Email Domains to be used during authorization.  

Before you begin this section, you will need to have the below information 

Token Server Resource Name (Also referred below as tokenserver> : you can run the following command to get the Token Server Resource Name
  
    HexaEight_Token_Issuer --showowner
    
Client ID : You will need to generate a Client ID for your Application using the following Command. Make note of the Client ID once the command has completed running succeessfully.

    HexaEight_Token_Issuer --clientid

---

Policy File Definitions:

	1.	Model.conf - Contains the Custom Access Model Definition for Enforcers. 
	2.	userpolicy.csv	- Contains Policy Enforement related to Users (Classified as anyonee possessing an email address and able to login using HexaEight Autheticator Mobile App)
	3.	resourcepolicy.csv - Contains Policy Enforcement related to Resource Servers (Servers are classified to not have an email address but possess a domain or generic name) 
	4.	clientappspolicy.csv - Contains Policy Enforcement about the user agent domains and apps that can use the token server to login to the application.
	5.	clientscopespolicy.csv - Contains Policy Enforcement about the users/servers and the roles


Quick Start:


The policy entry required in each of the above mentioned policy file are based on the below four questions. 

Note : The fourth field in the sample policy files are CONSTANTS and should NOT be changed. 


### 1. Who are the email users allowed to Login To Your Application Through the Token Server?

Create a policy entry per line if allowing only specific domains or use Wildcard(*) to allow all users from a domain like shown below

**Policy File Name : userpolicy.csv**

```
# Allow all email users to login to Application
# p, /*@*.*, mytoken.server, CLIENTID, login
# p, /*@*.*, mytoken.server, CLIENTID, ask
# p, /*@*.*, mytoken.server, CLIENTID, poke

# Allow any user who has an email address from domain.dom to login to Application
# p, /*@*.domain.dom, mytoken.server, CLIENTID, login
# p, /*@*.domain.dom, mytoken.server, CLIENTID, ask
# p, /*@*.domain.dom, mytoken.server, CLIENTID, poke

```
During Login process, the user is expected to solve a Captcha. The below policy entry defines, the list of email users who can request a captcha from the Token Server to login to the application.

**Policy File Name : captchapolicy.csv**

```
# Allow all email users to fetch a captcha
# p, /*@/*./*, mytoken.server, CAPTCHA, enable

# Add a deny rule if you want to disallow specific email users or domains from logging in, you can skip this entry if you dont have such a requirement.
# p,/*@baddomain.dom, mytoken.server, CAPTCHA, deny

```

Also at the time of login, users are prompted with a list of SCOPES, these SCOPES directly map to roles that a user should belong inside the application. 

If you want to allow only specific users with roles then you can add a enforcement either at user or at a domain level to allow users to choose specific roles while logging into the application.

**Policy File Name : clientscopespolicy.csv**
```
# Allow all users to login using a role called DEFAULT.
# p, /*@*.*, /*, CLIENTID, DEFAULT

# Allow any mail user whose mail ends with _admin to login using the ADMIN role
# p, /*_admin@myemail.dom, /*, CLIENTID, ADMIN

```


### 2. Upon successful login, can an email user communicate with another email user who is also logged into your application. 

If the answer to the above question is yes, the create a policy entry per line like shown below. Use wildcard(*) to allow specific domains.

If you do not want any email users to communicate with any other email user, do not define any policy entries.

```
# Allow any email user who has logged into to Application to communicate with any other email user
# p, /*@/*./*, /*@/*./*, CLIENTID, ask
# p, /*@/*./*, /*@/*./*, CLIENTID, poke


# Below example shows that logged in users can only communicate with a email user who is part of support
# p, support@mydomain.com, /*@/*./*, CLIENTID, ask
# p, support@mydomain.com, /*@/*./*, CLIENTID, poke
# p, /*@/*./*, support@mydomain.com, CLIENTID, ask
# p, /*@/*./*, support@mydomain.com, CLIENTID, poke


```

### 3. Upon successful login can any email users communicate with resource servers which are part of the application?

If the answer to the above question is yes, the create a policy entry per line like shown below. Use wildcard(*) to for multiple resource servers belonging to same domain.

**Policy File Name : userpolicy.csv**

```
# Allow any email server to establish direct communication with multiple resource servers.
# p, /*@/*./*, myresource.server, CLIENTID, ask
# p, /*@/*./*, myresource.server, CLIENTID, poke
# p, /*@/*./*, myresource2.server, CLIENTID, ask
# p, /*@/*./*, myresource2.server, CLIENTID, poke

# Allow any email server to establish direct communication with all resource servers part of the Application
# p, /*@/*./*, /*, CLIENTID, ask
# p, /*@/*./*, /*, CLIENTID, poke
```


### 4. Which Resource Servers are allowed to login and can they communicate with email users or other resource servers inside the application?

Unlike the previous three questions, this section pertains to Resource Servers.

**Policy File Name : resourcepolicy.csv**

```
# Allow myresorce.server to login to my token server
# p, myresource.server, mytoken.server, CLIENTID, login


# Allow myresorce.server to communicate with any other email user inside th application.
# p, myresource.server, /*@/*./*, CLIENTID, ask
# p, myresource.server, /*@/*./*, CLIENTID, poke

# Allow myresorce.server to communicate with another resource server called customresource.server and vice versa from within the application.
# p, myresource.server, customresource.server, CLIENTID, ask
# p, myresource.server, customresource.server, CLIENTID, poke

# p, customresource.server, myresource.server, CLIENTID, ask
# p, customresource.server, myresource.server, CLIENTID, poke

```

**Note : Resource Serves DO NOT Use Captcha to login to the application, so there is no entry requeird in captchapolicy.csv file.

There are no changes required to model.conf and captchamodel.conf files.

Once configuration entries are created, copy these files into the same directory where HexaEight Token Server is installed.

Start the Token Server.
