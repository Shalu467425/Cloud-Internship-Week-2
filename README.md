# Cloud-Internship-Week-2

Documentation of Cloud Internship Week # 2 Tasks

Introduction to Microsoft Azure: Describe Azure architecture and services

Level
Beginner

Product
Azure

Role
AdministratorDeveloperDevOps EngineerSolution Architect

Subject
ArchitectureCloud computingTechnical infrastructure

Exercise # 1 : Explore the Learn sandbox

Task 1: Use the PowerShell CLI

Task 2: Use the BASH CLI

Task 3: Use Azure CLI interactive mode

Task 3: Use Azure CLI interactive mode

The above exercise was carried in sandbox azure

Exercise # 2- Create an Azure resource

Exercise - Create an Azure resource

1 Sign in to the Azure portal.
2 Select Create a resource > Virtual Machine > Create.
3 The Create a virtual machine pane opens to the basics tab.
4 Verify or enter the following values for each setting. If a setting isn’t specified, leave the default value.
5 Select Review and Create.
6 Select Create

Task 2: Verify resources created

1 Select Home.
2 Select Resource groups.
3 Select the [sandbox resource group name] resource group.

Clean up - automatic by sandbox

Exercise # 3 : Create an Azure virtual machine

Task # 1 : Task 1: Create a Linux virtual machine and install Nginx

1 From Cloud Shell, run the following az vm create command to create a Linux VM:

az vm create \
  --resource-group "learn-dda93b6b-4853-4cef-83db-5ab31ab6526d" \
  --name my-vm \
  --public-ip-sku Standard \
  --image Ubuntu2204 \
  --admin-username azureuser \
  --generate-ssh-keys

2 Run the following az vm extension set command to configure Nginx on your VM:

az vm extension set \
  --resource-group "learn-dda93b6b-4853-4cef-83db-5ab31ab6526d" \
  --vm-name my-vm \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --version 2.1 \
  --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"]}' \
  --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'
 

To summarize, the script:
a. Runs apt-get update to download the latest package information from the internet. This step helps ensure that the next command can locate the latest version of the Nginx package.
b. Installs Nginx.
c. Sets the home page, /var/www/html/index.html, to print a welcome message that includes your VM's host name.

Exercise # 4 - Configure network access

Task 1: Access your web server

1 Run the following az vm list-ip-addresses command to get your VM's IP address and store the result as a Bash variable:
IPADDRESS="$(az vm list-ip-addresses \ 
--resource-group "learn-dda93b6b-4853-4cef-83db-5ab31ab6526d" \
--name my-vm \
--query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \ 
--output tsv)"  

2. Run the following curl command to download the home page:

curl --connect-timeout 5 http://$IPADDRESS

3. As an optional step, try to access the web server from a browser:

a. Run the following to print your VM's IP address to the console:

echo $IPADDRESS 

You see an IP address, for example, 20.253.160.185

b. Copy the IP address that you see to the clipboard.

c. Open a new browser tab and go to your web server. After a few moments, you see that the connection isn't happening. If you wait for the browser to time out, you see something like this:

screen Shot of Web Browser

d.Keep this browser tab open for later.



Task 2: List the current network security group rules

Task 3: Create the network security rule

Task 4: Access your web server again

