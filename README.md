# ☁️ Cloud Internship — Week 2

![Azure](https://img.shields.io/badge/Microsoft%20Azure-0078D4?logo=microsoftazure&logoColor=white)
![Beginner Level](https://img.shields.io/badge/Level-Beginner-green)
![Role](https://img.shields.io/badge/Role-Administrator%20%7C%20Developer%20%7C%20DevOps%20Engineer%20%7C%20Solution%20Architect-blue)
![Subject](https://img.shields.io/badge/Subject-Cloud%20Computing%20%7C%20Azure%20Architecture%20%7C%20Infrastructure-orange)

## 📖 Overview
This document records the **Week 2** tasks completed during the Cloud Internship, focusing on exploring the Microsoft Azure environment, creating resources, configuring networking, and managing storage.

---

## 🏗 Introduction to Microsoft Azure
Microsoft Azure is a cloud computing platform that provides a wide range of services, including:
- **Compute** (Virtual Machines, Functions, App Services)
- **Networking** (Virtual Networks, Load Balancers, Firewalls)
- **Storage** (Blob, Table, Queue, File)
- **Databases** (SQL Database, Cosmos DB, PostgreSQL)
- **AI & Analytics** (Cognitive Services, Synapse Analytics)

The architecture typically consists of:
1. **Frontend** — Azure Portal, CLI, SDKs, and APIs for user interaction.
2. **Control Plane** — Manages resources, authentication, and orchestration.
3. **Data Plane** — Handles the actual operations (e.g., reading/writing data, serving applications).

---

## 📌 Exercises

### **Exercise 1 — Explore the Learn Sandbox**
- **Task 1:** Use the PowerShell CLI  
- **Task 2:** Use the Bash CLI  
- **Task 3:** Use Azure CLI Interactive Mode  

_All tasks were performed in the Azure Learn Sandbox._

---

### **Exercise 2 — Create an Azure Resource**
**Task 1:** Create a Virtual Machine in Azure Portal
1. Sign in to the Azure portal.
2. **Create a Resource** → Select **Virtual Machine** → **Create**.
3. Fill in basic settings, leaving unspecified values at default.
4. Select **Review + Create** → **Create**.

**Task 2:** Verify Resources
1. Navigate to **Home** → **Resource Groups**.
2. Select the sandbox resource group.
3. Confirm the created resources are listed.

_Cleanup is done automatically by the sandbox._

---

### **Exercise 3 — Create an Azure Virtual Machine (CLI)**
**Task:** Deploy a Linux Virtual Machine
```bash
az vm create \
  --resource-group "<sandbox-resource-group>" \
  --name my-vm \
  --public-ip-sku Standard \
  --image Ubuntu2204 \
  --admin-username azureuser \
  --generate-ssh-keys

Exercise 4 — Configure Network Access
Task 1: Retrieve VM Public IP

IPADDRESS="$(az vm list-ip-addresses \
--resource-group "<sandbox-resource-group>" \
--name my-vm \
--query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
--output tsv)"

echo $IPADDRESS
Use curl to test connectivity:

curl --connect-timeout 5 http://$IPADDRESS
Task 2: View Current Network Security Rules

az network nsg list \
  --resource-group "<sandbox-resource-group>" \
  --query '[].name' --output tsv

az network nsg rule list \
  --resource-group "<sandbox-resource-group>" \
  --nsg-name my-vmNSG \
  --query '[].{Name:name, Priority:priority, Port:destinationPortRange, Access:access}' \
  --output table
Task 3: Allow HTTP Traffic

az network nsg rule create \
  --resource-group "<sandbox-resource-group>" \
  --nsg-name my-vmNSG \
  --name allow-http \
  --protocol tcp \
  --priority 100 \
  --destination-port-range 80 \
  --access Allow

Verify:

az network nsg rule list \
  --resource-group "<sandbox-resource-group>" \
  --nsg-name my-vmNSG \
  --query '[].{Name:name, Priority:priority, Port:destinationPortRange, Access:access}' \
  --output table
Exercise 5 — Create and Manage Storage Blob
Task 1: Create a Storage Account

Azure Portal → Create a Resource → Storage Account → Create.

Fill in:

Subscription: Concierge Subscription

Resource Group: Sandbox resource group

Storage Account Name: Unique name

Performance: Standard

Redundancy: Locally Redundant Storage (LRS)

Anonymous Access: Enabled (on individual containers)

Review + Create → Create → Go to Resource.

Task 2: Create a Blob Container and Upload a File

Data Storage → Containers → + Container (set access to Private).

Upload file via Upload option.

Attempt to open the blob URL (expect access denied).

Task 3: Change Blob Access Level

Select the container → Change Access Level → Blob (Anonymous Read Access) → OK.

Refresh blob URL to verify public access.

🧹 Cleanup
Sandbox automatically deletes all created resources.

In personal subscriptions, remove unused resources to avoid charges:

az group delete --name <resource-group-name> --yes --no-wait
✅ End of Week 2 Documentation

---

