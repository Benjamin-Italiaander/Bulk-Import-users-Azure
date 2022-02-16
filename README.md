# Bulk Import users into AzureAD or ExchangeOnline Security groups and Mail-enabled-Security-groups
This is a small tutorial how to bulk add users to AzureAD security groups and Exchange Online Mail-enabled-Security-groups.


###First export a security group of create a list of names by creating a flie with the content showed below and save this file as c:\Users\myuser\users.csv
 
```
UserPrincipalName
-----------------
user1@domain.tld
user2@domain.tld
user2@domain.tld
user2@domain.tld
```

#Open evilated-powershell (powershell as administrator) i prefer to use ISE-powershell

#First install/import the ExchangeOnlineManagement module
Import-Module ExchangeOnlineManagement

#check installed version, this must be at least Version 2
Get-Module ExchangeOnlineManagement


#Connect ot Office365 Exchane Online
Connect-ExchangeOnline -UserPrincipalName username@domain.tld

#There will be a pop-up to login with user credentials.

#Now check if the import the users.csv file works, you will see the list of the users in this file.
Import-Csv C:\Users\myuser\users.csv

#If the check shows all users you can load the file into memory variable $Users
$Users = Import-Csv  C:\Users\italiaander\users.csv


#Import all users to the SecurityGroup (replace YourSecurityGroupName with a existing securitygroup) and copy past this

foreach ($User in $Users) {
    # Retrieve UPN
    $UPN = $User.UserPrincipalName


   
   Add-DistributionGroupMember -Identity "YourSecurityGroupName" -Member $UPN
   }








#check if import worked by
Get-DistributionGroupMember -Identity "YourSecurityGroupName"

