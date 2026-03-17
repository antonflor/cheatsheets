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
  - *Azure Container Instances (ACI):* Run containers without managing servers.
  - *Azure Functions:* Serverless compute for event-driven workloads.
- **Storage Services:**
  - *Azure Blob Storage:* Object storage solution for unstructured data.
  - *Azure Disk Storage:* High-performance disk storage for VMs.
  - *Azure File Storage:* Managed file shares accessible via SMB/NFS.
  - *Azure Queue Storage:* Messaging between application components.
- **Networking Services:**
  - *Azure Virtual Network (VNet):* Establish private networks in the cloud.
  - *Azure Load Balancer:* Distribute incoming network traffic.
  - *Azure Application Gateway:* Application-level routing and load balancing.
  - *Azure VPN Gateway:* Site-to-site and point-to-site VPN connectivity.
  - *Azure ExpressRoute:* Dedicated private connectivity to Azure.
- **Database Services:**
  - *Azure SQL Database:* Managed relational database service.
  - *Azure Cosmos DB:* Globally distributed, multi-model database.
  - *Azure Database for MySQL/PostgreSQL:* Managed open-source database services.

## 3. Security, Privacy, Compliance, and Trust

- **Azure Active Directory (AD):** Cloud-based identity and access management service.
- **Role-Based Access Control (RBAC):** Manage user access to Azure resources.
- **Azure Security Center / Microsoft Defender for Cloud:** Unified security management system.
- **Azure Key Vault:** Manage and safeguard cryptographic keys and secrets.
- **Azure DDoS Protection:** Protect against distributed denial-of-service attacks.
- **Microsoft Sentinel:** Cloud-native SIEM and SOAR solution.

## 4. Azure Pricing and Support

- **Pricing Models:**
  - *Consumption-Based:* Pay-as-you-go based on actual usage.
  - *Reserved Instances:* Commit to 1 or 3 years for discounted rates.
  - *Spot Instances:* Use unused Azure capacity at deep discounts.
- **Total Cost of Ownership (TCO) Calculator:** Estimate cost savings by migrating to Azure.
- **Azure Pricing Calculator:** Estimate costs for specific Azure configurations.
- **Service Level Agreements (SLAs):** Understand uptime guarantees for Azure services.
- **Support Plans:**
  - *Basic:* Free support for billing and subscription issues.
  - *Developer:* For trial and non-production environments.
  - *Standard:* For production workloads.
  - *Professional Direct:* For business-critical dependence.
  - *Premier:* Enterprise-level proactive support.

## 5. Azure Management Tools

- **Azure Portal:** Web-based interface for managing Azure services.
- **Azure CLI:** Command-line tool for managing Azure resources.
- **Azure PowerShell:** PowerShell module for Azure management.
- **Azure Resource Manager (ARM):** Deployment and management service for Azure.
- **Azure Bicep:** Domain-specific language for deploying Azure resources (simpler than ARM JSON).
- **Azure Cloud Shell:** Browser-based shell with Azure CLI and PowerShell pre-installed.

---

## Azure Command-Line Interface (CLI)

Azure CLI is a command-line tool for managing Azure resources. Below are common commands:

### Login and Account Management

```bash
# Login interactively
az login

# Login with a service principal
az login --service-principal -u [APP_ID] -p [PASSWORD] --tenant [TENANT_ID]

# List subscriptions
az account list --output table

# Set active subscription
az account set --subscription [SUBSCRIPTION_ID]

# Show current account
az account show
```

### Resource Groups

```bash
# Create a Resource Group
az group create --name MyResourceGroup --location eastus

# List Resource Groups
az group list --output table

# Show a specific Resource Group
az group show --name MyResourceGroup

# Delete a Resource Group
az group delete --name MyResourceGroup --yes --no-wait
```

### Virtual Machines

```bash
# Create a VM
az vm create \
  --resource-group MyResourceGroup \
  --name MyVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys

# List VMs
az vm list --output table

# Start a VM
az vm start --resource-group MyResourceGroup --name MyVM

# Stop (deallocate) a VM
az vm stop --resource-group MyResourceGroup --name MyVM

# Restart a VM
az vm restart --resource-group MyResourceGroup --name MyVM

# Delete a VM
az vm delete --resource-group MyResourceGroup --name MyVM --yes

# Get VM details
az vm show --resource-group MyResourceGroup --name MyVM

# List VM sizes in a region
az vm list-sizes --location eastus --output table

# Open a port on a VM
az vm open-port --resource-group MyResourceGroup --name MyVM --port 80
```

### Storage Accounts

```bash
# Create a Storage Account
az storage account create \
  --name mystorageaccount \
  --resource-group MyResourceGroup \
  --location eastus \
  --sku Standard_LRS

# List Storage Accounts
az storage account list --resource-group MyResourceGroup --output table

# Show Storage Account keys
az storage account keys list --account-name mystorageaccount --resource-group MyResourceGroup

# Delete a Storage Account
az storage account delete --name mystorageaccount --resource-group MyResourceGroup --yes

# Upload a file to Blob Storage
az storage blob upload \
  --account-name mystorageaccount \
  --container-name mycontainer \
  --name myblob \
  --file ./localfile.txt

# List blobs in a container
az storage blob list --account-name mystorageaccount --container-name mycontainer --output table
```

### Azure Kubernetes Service (AKS)

```bash
# Create an AKS Cluster
az aks create \
  --resource-group MyResourceGroup \
  --name MyAKSCluster \
  --node-count 2 \
  --enable-addons monitoring \
  --generate-ssh-keys

# List AKS Clusters
az aks list --output table

# Get AKS Credentials (configure kubectl)
az aks get-credentials --resource-group MyResourceGroup --name MyAKSCluster

# Scale AKS Cluster node count
az aks scale --resource-group MyResourceGroup --name MyAKSCluster --node-count 3

# Upgrade AKS Cluster
az aks upgrade --resource-group MyResourceGroup --name MyAKSCluster --kubernetes-version [VERSION]

# Show AKS Cluster details
az aks show --resource-group MyResourceGroup --name MyAKSCluster --output table

# Stop AKS Cluster (save costs when not in use)
az aks stop --resource-group MyResourceGroup --name MyAKSCluster

# Start AKS Cluster
az aks start --resource-group MyResourceGroup --name MyAKSCluster

# Delete AKS Cluster
az aks delete --resource-group MyResourceGroup --name MyAKSCluster --yes
```

### Networking

```bash
# Create a Virtual Network
az network vnet create \
  --resource-group MyResourceGroup \
  --name MyVNet \
  --address-prefix 10.0.0.0/16

# Create a Subnet
az network vnet subnet create \
  --resource-group MyResourceGroup \
  --vnet-name MyVNet \
  --name MySubnet \
  --address-prefixes 10.0.1.0/24

# Create a Network Security Group
az network nsg create --resource-group MyResourceGroup --name MyNSG

# Add an NSG rule
az network nsg rule create \
  --resource-group MyResourceGroup \
  --nsg-name MyNSG \
  --name AllowSSH \
  --protocol tcp \
  --priority 1000 \
  --destination-port-range 22 \
  --access Allow

# Create a Public IP
az network public-ip create --resource-group MyResourceGroup --name MyPublicIP

# List VNets
az network vnet list --output table
```

### Azure Active Directory / Entra ID

```bash
# List users
az ad user list --output table

# Create a user
az ad user create --display-name "John Doe" --password [PASSWORD] --user-principal-name john@example.com

# List service principals
az ad sp list --output table

# Create a service principal
az ad sp create-for-rbac --name MyServicePrincipal --role Contributor --scopes /subscriptions/[SUBSCRIPTION_ID]

# List role assignments
az role assignment list --output table
```

### Tips for Using Azure CLI

- **Output Formatting**: Use `--output` (`-o`) with `json`, `jsonc`, `table`, `tsv`, or `yaml` for different output formats.
- **Queries**: Use `--query` with JMESPath syntax to filter output: `az vm list --query "[].{Name:name, Location:location}" --output table`
- **Help**: Use `az [command] --help` for detailed documentation on any command.
- **Version Update**: Keep Azure CLI updated: `az upgrade`
- **Interactive Mode**: Use `az interactive` for an auto-complete shell experience.
