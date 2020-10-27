Install IIS on Azure using Remote Powershell
============================================

            

This Script was created to Install IIS **on Azure VM
**using Remote PowerShell. The belowscript is for IIS and Windows Server 2012\2012 R2 & Windows 2016 on Azure VM.



Simply edit the PS1 file based on the comments in the file to match your environment and you will have a simple way of deploying IIS.


Be sure to take note of the comments in the PS1 file to insert your input according to your environment.


 



PowerShell
Edit|Remove
powershell
## Enter your domain name e.g. azureadmin 
## Enter your admin password e.g. P@ssw0rd
## Enter svrname = server name that you will deploy IIS on it e.g. Server1 
## Enter servicename = Cloud service name that you will deploy IIS on it e.g. Server1 

$adminname  = '<username>'
$adminPassword = '<password>'

$svrname = 'Server1'
$servicename =  'Server1'

Function Install-IIS ($svrname, $servicename)

{

# Install the WinRM Certificate first to access the VM via Remote PS
Install-WinRMCertificateForVM $servicename $svrname

# Return back the correct URI for Remote PowerShell
$uri = Get-AzureWinRMUri -ServiceName $servicename -Name $svrname
$SecurePassword = $adminpassword | ConvertTo-SecureString -AsPlainText -Force
$credential = new-object -typename System.Management.Automation.PSCredential -argumentlist $adminname,$SecurePassword

# Install IIS
Invoke-Command -ConnectionUri $uri.ToString() -Credential $credential -ScriptBlock {
  Install-WindowsFeature -Name Web-Server -IncludeManagementTools -Source C:\Windows\WinSxS
}

# Disable Windows Firewall:
Invoke-Command -ConnectionUri $uri.ToString() -Credential $credential -ScriptBlock {
  Set-NetFirewallProfile -All -Enabled False 
}

}


## Enter your domain name e.g. azureadmin  
## Enter your admin password e.g. P@ssw0rd 
## Enter svrname = server name that you will deploy IIS on it e.g. Server1  
## Enter servicename = Cloud service name that you will deploy IIS on it e.g. Server1  
 
$adminname  = '<username>' 
$adminPassword = '<password>' 
 
$svrname = 'Server1' 
$servicename =  'Server1' 
 
Function Install-IIS ($svrname, $servicename) 
 
{ 
 
# Install the WinRM Certificate first to access the VM via Remote PS 
Install-WinRMCertificateForVM $servicename $svrname 
 
# Return back the correct URI for Remote PowerShell 
$uri = Get-AzureWinRMUri -ServiceName $servicename -Name $svrname 
$SecurePassword = $adminpassword | ConvertTo-SecureString -AsPlainText -Force 
$credential = new-object -typename System.Management.Automation.PSCredential -argumentlist $adminname,$SecurePassword 
 
# Install IIS 
Invoke-Command -ConnectionUri $uri.ToString() -Credential $credential -ScriptBlock { 
  Install-WindowsFeature -Name Web-Server -IncludeManagementTools -Source C:\Windows\WinSxS 
} 
 
# Disable Windows Firewall: 
Invoke-Command -ConnectionUri $uri.ToString() -Credential $credential -ScriptBlock { 
  Set-NetFirewallProfile -All -Enabled False  
} 
 
} 





 


You can use this script in following steps:


  *  Download the script and copy it to your Server. 

  *  Open PowerShell as Administrator and Connect to your Azure Tenant

  *  Then Run .\Install-IIS.ps1 

    
TechNet gallery is retiring! This script was migrated from TechNet script center to GitHub by Microsoft Azure Automation product group. All the Script Center fields like Rating, RatingCount and DownloadCount have been carried over to Github as-is for the migrated scripts only. Note : The Script Center fields will not be applicable for the new repositories created in Github & hence those fields will not show up for new Github repositories.
