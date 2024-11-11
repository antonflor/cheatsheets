# Azure Cheat Sheet

## Overview

Microsoft Azure is a comprehensive cloud platform offering a wide array of services for computing, storage, networking, and more. This cheat sheet provides a concise reference to essential Azure services and commands.

---

# Azure Fundamentals (AZ-900) Cheat Sheet

## 1. Cloud Concepts

- **Cloud Computing:** Understand the basics of cloud computing, including its benefits such as scalability, reliability, and cost-efficiency.
- **Cloud Models:**
  - *Public Cloud:* Services offered over the public internet.
  - *Private Cloud:* Dedicated services for a single organization.
  - *Hybrid Cloud:* Combination of public and private clouds.
- **Cloud Service Types:**
  - *Infrastructure as a Service (IaaS):* Provides virtualized computing resources.
  - *Platform as a Service (PaaS):* Offers hardware and software tools over the internet.
  - *Software as a Service (SaaS):* Delivers software applications over the internet.

## 2. Core Azure Services

- **Compute Services:**
  - *Azure Virtual Machines:* Deploy and manage virtual machines.
  - *Azure App Service:* Host web apps and RESTful APIs.
  - *Azure Kubernetes Service (AKS):* Manage Kubernetes clusters.
- **Storage Services:**
  - *Azure Blob Storage:* Object storage solution.
  - *Azure Disk Storage:* High-performance disk storage.
  - *Azure File Storage:* Managed file shares.
- **Networking Services:**
  - *Azure Virtual Network (VNet):* Establish private networks.
  - *Azure Load Balancer:* Distribute incoming network traffic.
  - *Azure Application Gateway:* Application-level routing and load balancing.
- **Database Services:**
  - *Azure SQL Database:* Managed relational database service.
  - *Azure Cosmos DB:* Globally distributed, multi-model database.
  - *Azure Database for MySQL/PostgreSQL:* Managed open-source database services.

## 3. Security, Privacy, Compliance, and Trust

- **Azure Active Directory (AD):** Cloud-based identity and access management service.
- **Role-Based Access Control (RBAC):** Manage user access to Azure resources.
- **Azure Security Center:** Unified security management system.
- **Azure Key Vault:** Manage and safeguard cryptographic keys and secrets.

## 4. Azure Pricing and Support

- **Pricing Models:**
  - *Consumption-Based:* Pay-as-you-go.
  - *Reserved Instances:* Commit to a period for discounted rates.
- **Total Cost of Ownership (TCO) Calculator:** Estimate cost savings by migrating to Azure.
- **Service Level Agreements (SLAs):** Understand uptime guarantees for Azure services.
- **Support Plans:**
  - *Basic:* Free support for billing and subscription issues.
  - *Developer:* For trial and non-production environments.
  - *Standard:* For production workloads.
  - *Professional Direct:* For business-critical dependence.

## 5. Azure Management Tools

- **Azure Portal:** Web-based interface for managing Azure services.
- **Azure CLI:** Command-line tool for managing Azure resources.
- **Azure PowerShell:** PowerShell module for Azure management.
- **Azure Resource Manager (ARM):** Deployment and management service for Azure.

---

## Azure Command-Line Interface (CLI)

Azure CLI is a command-line tool for managing Azure resources. Below are common commands:

### Login

```
az login
```

### Resource Groups
- Create a Resource Group:
```
az group create --name MyResourceGroup --location eastus
```
- List Resource Groups:
```
az group list --output table
```
- Delete a Resource Group:
```
az group delete --name MyResourceGroup --yes --no-wait
```

### Virtual Machines
- Create a VM:
```
az vm create --resource-group MyResourceGroup --name MyVM --image UbuntuLTS --admin-username azureuser --generate-ssh-keys
```
- Start a VM:
```
az vm start --resource-group MyResourceGroup --name MyVM
```
- Stop a VM:
```
az vm stop --resource-group MyResourceGroup --name MyVM
```
- Delete a VM:
```
az vm delete --resource-group MyResourceGroup --name MyVM --yes
```

### Storage Accounts
- Create a Storage Account:
```
az storage account create --name mystorageaccount --resource-group MyResourceGroup --location eastus --sku Standard_LRS
```
- List Storage Accounts:
```
az storage account list --resource-group MyResourceGroup --output table
```
- Delete a Storage Account:
```
az storage account delete --name mystorageaccount --resource-group MyResourceGroup --yes
```

### Azure Kubernetes Service (AKS)
- Create an AKS Cluster:
```
az aks create --resource-group MyResourceGroup --name MyAKSCluster --node-count 1 --enable-addons monitoring --generate-ssh-keys
```
- Get AKS Credentials:
```
az aks get-credentials --resource-group MyResourceGroup --name MyAKSCluster
```
- Scale AKS Cluster:
```
az aks scale --resource-group MyResourceGroup --name MyAKSCluster --node-count 3
