# ☁️ Cloud Internship – Week 2

[![Level](https://img.shields.io/badge/Level-Beginner-brightgreen)]()
[![Product](https://img.shields.io/badge/Product-Azure-blue)]()
[![Role](https://img.shields.io/badge/Role-Administrator%20%7C%20Developer%20%7C%20DevOps%20Engineer%20%7C%20Solution%20Architect-purple)]()
[![Subject](https://img.shields.io/badge/Subject-Cloud%20Computing%20%7C%20Architecture%20%7C%20Technical%20Infrastructure-orange)]()

> **Description:**  
> Documentation of **Week 2 Cloud Internship** tasks focused on *Introduction to Microsoft Azure* — exploring Azure architecture, services, and hands-on labs in the Microsoft Learn Sandbox.

---

## 📚 Table of Contents
- [Exercise 1 – Explore the Learn Sandbox](#exercise-1)
- [Exercise 2 – Create an Azure Resource](#exercise-2)
- [Exercise 3 – Create a Linux VM and Install Nginx](#exercise-3)
- [Exercise 4 – Configure Network Access](#exercise-4)
- [Exercise 5 – Create and Configure a Storage Blob](#exercise-5)
- [Cleanup Notes](#cleanup-notes)

---

## <a name="exercise-1"></a>Exercise 1 – Explore the Learn Sandbox
<details>
<summary>▶ View Details</summary>

**Tasks:**
1. Use the **PowerShell CLI**
2. Use the **Bash CLI**
3. Use **Azure CLI Interactive Mode**

📌 *This exercise was performed entirely in the Azure Sandbox environment.*

🖼 *[Screenshot Placeholder – CLI in action]*

</details>

---

## <a name="exercise-2"></a>Exercise 2 – Create an Azure Resource
<details>
  
**Steps:**
1. Sign in to the Azure Portal.
2. Navigate to **Create a resource → Virtual Machine → Create**.
3. Fill in the required values in the *Basics* tab.
4. Click **Review + Create** → **Create**.

### Verify Resources
1. Go to **Home → Resource Groups**.
2. Select the sandbox-created resource group.
   
</details>

## <a name="exercise-3"></a>Exercise 3 – Create a Linux VM and Install Nginx
<details>
<summary>▶ View Details</summary>

### Create VM
```bash
az vm create \
  --resource-group "learn-dda93b6b-4853-4cef-83db-5ab31ab6526d" \
  --name my-vm \
  --public-ip-sku Standard \
  --image Ubuntu2204 \
  --admin-username azureuser \
  --generate-ssh-keys

Install & Configure Nginx

az vm extension set \
  --resource-group "learn-dda93b6b-4853-4cef-83db-5ab31ab6526d" \
  --vm-name my-vm \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --version 2.1 \
  --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"]}' \
  --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'

</details>

## <a name="exercise-4"></a>Exercise 4 – Configure Network Access

<details>

<summary>▶ View Details</summary>

Get VM IP

IPADDRESS="$(az vm list-ip-addresses \
--resource-group "learn-dda93b6b-4853-4cef-83db-5ab31ab6526d" \
--name my-vm \
--query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
--output tsv)"

echo $IPADDRESS
Test Connection

curl --connect-timeout 5 http://$IPADDRESS

List NSG Rules

az network nsg list \
  --resource-group "learn-7dc5d339-701e-4c92-9906-832b73c8d617" \
  --query '[].name' \
  --output tsv

az network nsg rule list \
  --resource-group "learn-7dc5d339-701e-4c92-9906-832b73c8d617" \
  --nsg-name my-vmNSG \
  --query '[].{Name:name, Priority:priority, Port:destinationPortRange, Access:access}' \
  --output table
Create HTTP Rule

az network nsg rule create \
  --resource-group "learn-7dc5d339-701e-4c92-9906-832b73c8d617" \
  --nsg-name my-vmNSG \
  --name allow-http \
  --protocol tcp \
  --priority 100 \
  --destination-port-range 80 \
  --access Allow

</details>

## <a name="exercise-5"></a>Exercise 5 – Create and Configure a Storage Blob

<details> 

| Setting                    | Value                           |
| -------------------------- | ------------------------------- |
| Subscription               | Concierge Subscription          |
| Resource Group             | Sandbox resource group          |
| Storage Account Name       | *Unique name*                   |
| Performance                | Standard                        |
| Redundancy                 | Locally Redundant Storage (LRS) |
| Anonymous Container Access | Enabled                         |

Create Container & Upload Blob
--Navigate to Data Storage → Containers → + Container

--Access Level: Private

--Upload a file

--Copy Blob URL & verify access

Change Blob Access Level
--Set Anonymous Access Level to Blob

-- Refresh browser tab to confirm public access

🧹 Cleanup Notes
--Sandbox cleans up automatically.
--In personal subscriptions:
--Delete unused resources
--Or delete entire resource group to save costs

</details> 

#Azure #CloudComputing #VirtualMachine #Nginx #StorageBlob #MicrosoftLearn #DevOps

