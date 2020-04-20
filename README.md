VMWare template automation using packer....

Prereqs:
-vCenter
-Azure DevOps account
-Azure DevOps on-prem agent

Things to install locally:

-Install packer.exe (v1.4.5) -- Add packer path to environment variables in Windows
-packer-builder-vsphere-iso.exe (v2.3)
-packer-provisioner-windows-update.exe (v0.7.1)
-Install VSCode
-Install Git and push local working code to Azure DevOps account


Files needed:

-WindowsServer.json (make sure to edit variables)
-variables.json (contains password info -- only needed if running locally instead of from AzureDevOps)

In Setup folder
-autounattend.xml (points to vmtools.ps1 that installs vmtools multiple times until fully installed)
-vmtools.ps1 (powershell script to install vmtools)
-setup.ps1 (enables WinRM)

If you need to make changes to the autounattend.xml file install the Windows ADK - Windows System Image Manager and load the xml file

ISOs needed on datastore:

-SW_DVD9_Win_Svr_STD_Core_and_DataCtr_Core_2016_64Bit_English_-3_MLF_X21-30350.ISO
-VMware-tools.iso

Run the following commands (if running locally):

C:\Users\Kevin\Documents\Automation Projects\Packer - Automate Windows Updates>packer validate .\windowsserver.json
C:\Users\Kevin\Documents\Automation Projects\Packer - Automate Windows Updates>packer build -var-file .\Variables.Json .\WindowsServer.Json 

(you may have to create the variables.json file since it is not used when running release from AzureDevOps)

--AzureDevOps build and release pipeline--

Steps involved:
-Use git on-prem to push working code from on-prem to AzureDevOps repo
-Configure an on-prem Azure Devops agent (under agent pools in AzureDevOps)
-Use a yaml file to setup a build in AzureDevOps
-Configure a pipeline and setup continuous integration (everytime you commit a code change, it triggers a build)
-Create a variables group (securely stores your passwords in Azure DevOps keeping passwords out of your code)
-Creating and scheduling a release
-Testing

To push local repository to Azure DevOps repository:
#In the local directory from the root of the project
git init
git remote add origin <URL for Azure Git repo>
git add .
git commit -m 'initial commit'
git push -u origin master

Please follow this guide if you need more details:
https://www.altaro.com/vmware/packer-pipelines-azure-devops/




