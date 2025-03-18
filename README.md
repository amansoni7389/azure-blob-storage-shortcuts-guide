# Creating a Shortcut from Azure Blob Storage Using Different Authentication Methods

## Introduction

This document provides a step-by-step guide on creating a shortcut from Azure Blob Storage using two different authentication methods:

- **Organizational Account** (Microsoft Entra ID)
- **Access Key Authentication**

## Prerequisites

- An active Azure Storage Account
- Microsoft Fabric workspace access
- Appropriate permissions based on the authentication method
- Azure CLI or Azure Portal for setup
- Hierarchical Namespace enabled on the ADLS Gen2 storage account

## Method 1: Creating a Shortcut Using Organizational Account Authentication

### Step 1: Assign Permissions

Ensure that the user or service principal has the required Azure RBAC roles assigned to the Storage Account:

- `Storage Blob Data Reader` (for read-only access)
- `Storage Blob Data Contributor` (for read and write access)
- `Storage Blob Data Owner` (full access including delete operations)

To assign permissions:

1. Go to Azure Portal → Storage Account
2. Navigate to Access Control (IAM)
3. Click **Add Role Assignment**
4. Choose one of the roles above and assign it to the user or service principal

### Step 2: Change Authentication Method for the Container

If you are accessing the container using Organizational Account, you need to change the authentication method of the container to Microsoft Entra ID authentication:

1. Go to Azure Portal → Storage Account → Containers
2. Click on the container you want to access
3. Navigate to Authentication Method
4. Change it to **Microsoft Entra ID authentication** (if not already set)
5. Save the changes

### Step 3: Create Shortcut in Microsoft Fabric

1. Open Microsoft Fabric
2. Go to Lakehouse → Shortcuts
3. Click **New Shortcut**
4. Select Azure Data Lake Storage (ADLS Gen2)
5. Choose **Organizational Account** as the authentication method
6. Sign in with your Microsoft Entra ID (formerly AAD) account
7. Select the container and folder from the Storage Account
8. Click **Create Shortcut**

### Troubleshooting

- Ensure your account has the required RBAC roles on the Storage Account
- Check Azure Storage firewall settings
- Verify that Microsoft Entra ID Authentication is enabled in your storage account settings
- If authentication fails, try re-signing into Microsoft Fabric and re-authorizing access
- Clear browser cache or try using Incognito mode
- Ensure Azure AD Conditional Access Policies are not blocking the authentication

## Method 2: Creating a Shortcut Using Access Key Authentication

### Step 1: Retrieve the Storage Account Key

1. Open Azure Portal
2. Navigate to Storage Accounts → Select your storage account
3. Go to Access keys under Security + networking
4. Copy one of the available keys

### Step 2: Ensure the Container Authentication Method is Set to Access Key

When using Access Key authentication, the container should use Access Key authentication:

1. Go to Azure Portal → Storage Account → Containers
2. Click on the container you want to access
3. Navigate to Authentication Method
4. Ensure it is set to **Access Key authentication** (this is the default setting)

### Step 3: Create Shortcut in Microsoft Fabric

1. Open Microsoft Fabric
2. Go to Lakehouse → Shortcuts
3. Click **New Shortcut**
4. Select Azure Data Lake Storage (ADLS Gen2)
5. Choose **Access Key** as the authentication method
6. Enter the storage account name and the copied access key
7. Select the container and folder
8. Click **Create Shortcut**

## Security Considerations

- ⚠️ Avoid sharing access keys as they provide full access
- 🔄 Rotate access keys periodically
- 🛡️ Use Organizational Account when possible for better security and access control

### Troubleshooting

- If authentication fails, double-check the copied access key
- Ensure Storage Account firewall settings allow your IP or the Microsoft Fabric service
- If the shortcut creation fails, try regenerating a new access key and retry
- Check the storage account logs for authentication failures
- If access is denied, verify that the container public access setting is not set to Private

## Conclusion

Both Organizational Account and Access Key authentication methods allow you to create shortcuts from Azure Blob Storage. **Organizational Account** is preferred for secure, role-based access, while **Access Key** provides a simpler but less secure alternative.

> **For best security practices, use Microsoft Entra ID authentication whenever possible.**

---

### Important Files & References:

- [Create ADLS Shortcut](https://learn.microsoft.com/en-us/fabric/onelake/create-adls-shortcut)
- [OneLake Shortcuts - ADLS Shortcuts](https://learn.microsoft.com/en-us/fabric/onelake/onelake-shortcuts)
- [Managing Access Keys in Azure Storage](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-keys-manage)
