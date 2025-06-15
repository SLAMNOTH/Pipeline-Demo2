[[![Build Status](https://dev.azure.com/juicttesting3/JUICT/_apis/build/status%2FSLAMNOTH.Pipeline-Demo2?branchName=main)](https://dev.azure.com/juicttesting3/JUICT/_build/latest?definitionId=4&branchName=main)](https://dev.azure.com/juicttesting3/JUICT/_apis/build/status%2FSLAMNOTH.Pipeline-Demo2?branchName=main)
# 🚀 Azure DevOps Bicep Deployment Pipeline

This repository contains an Azure DevOps pipeline that **builds, tests, deploys**, and optionally **cleans up** infrastructure using **Bicep** templates. The main goal is to provision a secure Windows VM in Azure.

---

## 📦 Pipeline Overview

```yaml
trigger:
- none
```

### 🔁 Stages in the Pipeline

| Stage     | Description                          |
|-----------|--------------------------------------|
| 🛠 Build   | Placeholder for build steps          |
| 🧪 Test    | Placeholder for future tests         |
| 🚀 Deploy  | Deploys infrastructure using Bicep  |
| 🧹 Cleanup | Deletes the resource group (optional) |

---

## 🗂️ Project Structure

- `main.bicep` – Infrastructure-as-Code definition
- Azure DevOps pipeline YAML – defines the CI/CD logic

---

## 🔐 Secret Management

- Username (`adminUN`) is defined as a pipeline variable
- Password (`adminPASS`) is securely fetched from **Azure Key Vault**
- Secrets are not hardcoded and follow best practices

---

## 🚀 Deploy Stage Explained

This stage:
- Fetches secrets from Azure Key Vault
- Creates the resource group (if it doesn’t exist)
- Deploys resources with `az deployment group create`

```bash
az deployment group create \
  --resource-group $(resourceGroupName) \
  --template-file $(templateFile) \
  --parameters adminUsername='$(adminUN)' adminPassword='$(adminPASS)'
```

---

## 🏗️ Infrastructure (Bicep)

Your `main.bicep` provisions the following Azure resources:

- Virtual Network & Subnet
- Network Security Group (NSG)
- Network Interface with Public IP
- Windows Virtual Machine

<details>
<summary>Example Bicep Snippet</summary>

```bicep
resource vm 'Microsoft.Compute/virtualMachines@2022-03-01' = {
  name: vmName
  location: location
  properties: {
    osProfile: {
      computerName: vmName
      adminUsername: adminUsername
      adminPassword: adminPassword
    }
    ...
  }
}
```

</details>

---

## 🧼 Cleanup Stage

The final stage deletes the entire resource group to:
- Keep your Azure environment clean
- Prevent extra costs
- Ensure ephemeral infrastructure

```bash
az group delete --name $(resourceGroupName) --yes --no-wait
```

⚠️ **Approval is required** before deletion!

---

## 🌍 Deployment Region

All resources are deployed in:

```
Location: West Europe (westeurope)
```

---

## 🔮 Future Improvements

- Add automated testing after deployment  
- Integrate with CI triggers (`push`/`pull_request`)  
- Include monitoring/alerts after VM deployment  

---

## 📷 Infrastructure Diagram (Simple)

```
[Internet] ──> [Public IP] ──> [NIC] ──> [VM]
                          │
                          └─> [NSG - Allow RDP (3389)]
```

---

