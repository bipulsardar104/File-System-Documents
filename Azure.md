# Microsoft Azure Portal: Step-by-Step Configuration Guide

This guide provides clear, sequential instructions for signing up to the Microsoft Azure Portal, creating a storage account, registering a service principal, assigning the required role, and retrieving essential credentials and endpoints. All actions are performed through the Azure Portal web interface at [portal.azure.com](https://portal.azure.com). Ensure you have a valid Microsoft account (personal or work/school) before proceeding.

## 1. Signing Up for the Microsoft Azure Portal

1. Open a web browser and navigate to [https://azure.microsoft.com/en-us/free/](https://azure.microsoft.com/en-us/free/).
2. Click **Start free** or **Create free account**.
3. Sign in with your Microsoft account. If you do not have one, click **Create one** and follow the prompts to register a new account.
4. Complete the identity verification process (phone number or credit card may be required for validation; no charges apply for the free tier during the initial period).
5. Once verified, you will be redirected to the Azure Portal dashboard at [portal.azure.com](https://portal.azure.com). Accept the terms and conditions to activate your subscription.

**Note:** New accounts receive a free Azure subscription with limited credits for the first 30 days and select services thereafter. Monitor usage via the **Cost Management** blade to avoid unexpected charges.

## 2. Creating a Storage Account

1. In the Azure Portal, click the **Search resources, services, and docs** bar at the top.
2. Type **Storage accounts** and select it from the results.
3. Click **+ Create**.
4. On the **Basics** tab, provide the following details:
   - **Subscription**: Select your active subscription.
   - **Resource group**: Click **Create new** and enter a descriptive name (e.g., `rg-storage-demo`).
   - **Storage account name**: Enter a globally unique name (3–24 characters, lowercase letters and numbers only).
   - **Region**: Select the geographic region closest to your location (e.g., `South India` for users in Andhra Pradesh).
   - **Performance**: Choose **Standard**.
   - **Redundancy**: Select **Locally-redundant storage (LRS)** for cost efficiency unless higher durability is required.
5. Click **Next** through the remaining tabs (**Advanced**, **Networking**, **Data protection**, **Encryption**, **Tags**) and accept default settings unless specific customizations are needed.
6. Review the configuration on the **Review + create** tab, then click **Create**.
7. Wait for deployment to complete (typically 1–2 minutes). Click **Go to resource** once the deployment succeeds.

## 3. Creating a Service Principal and Retrieving Client ID and Tenant ID

A service principal is an application identity used for automated authentication and authorization.

1. In the Azure Portal, search for **Microsoft Entra ID** and select it.
2. In the left-hand menu, under **Manage**, click **App registrations**.
3. Click **+ New registration**.
4. Enter the following:
   - **Name**: Provide a clear name (e.g., `sp-storage-access`).
   - **Supported account types**: Select **Accounts in this organizational directory only** (Single tenant) unless multi-tenant access is required.
   - **Redirect URI**: Leave blank (not required for this scenario).
5. Click **Register**.
6. On the **Overview** page of the newly created app registration, copy the following values immediately (they are displayed prominently):
   - **Application (client) ID** – This is your **Client ID**.
   - **Directory (tenant) ID** – This is your **Tenant ID**.

   > **Important:** Store these values securely. They will be required for authentication in scripts, applications, or tools.

## 4. Assigning the Storage Blob Data Contributor Role to the Service Principal

This role grants the service principal permissions to read, write, and delete data in Azure Blob Storage.

1. Navigate back to your storage account (search for it or use the resource navigation).
2. In the left-hand menu, under **Settings**, click **Access control (IAM)**.
3. Click **+ Add** > **Add role assignment**.
4. On the **Role** tab:
   - Search for and select **Storage Blob Data Contributor**.
   - Click **Next**.
5. On the **Members** tab:
   - Under **Assign access to**, select **User, group, or service principal**.
   - Click **+ Select members**.
   - In the search box, paste the **Client ID** or the name of the service principal created earlier.
   - Select the matching entry and click **Select**.
6. Click **Next**, then **Review + assign**.
7. Click **Review + assign** again to finalize.

The role assignment typically propagates within 30–60 seconds.

## 5. Retrieving Certificates & Secrets (Client Secret Value)

Client secrets are used for authentication alongside the Client ID and Tenant ID.

1. Return to **Microsoft Entra ID** > **App registrations**.
2. Select the service principal you created.
3. In the left-hand menu, under **Manage**, click **Certificates & secrets**.
4. On the **Client secrets** tab, click **+ New client secret**.
5. Provide a description (e.g., `storage-access-secret`) and choose an expiry period.
6. Click **Add**.
7. The **Value** column will display the secret (referred to as the **Client Secret Value** or **Client Value**).  
   **Copy this value immediately** – it will never be shown again after you leave the page.

   > **Security Note:** Store the Client Secret Value in a secure vault (e.g., Azure Key Vault) and never commit it to source control.

## 6. Locating the Blob Endpoint

The Blob endpoint is the base URL used to access the storage account programmatically.

1. Navigate to your storage account.
2. In the left-hand menu, click **Endpoints** (or view the **Overview** page).
3. Under **Blob service**, locate and copy the **Blob service primary endpoint**.  
   It will be in the format:  
   `https://<your-storage-account-name>.blob.core.windows.net`

This endpoint, combined with the Client ID, Tenant ID, and Client Secret Value, enables secure programmatic access to Blob Storage using the Azure SDKs or tools such as Azure CLI, PowerShell, or Terraform.

---

**Completion Summary**  
You have now configured a secure, role-based access setup for Azure Blob Storage. For production environments, consider using managed identities or certificates instead of client secrets for enhanced security. If you encounter any permission errors, verify that the service principal has been correctly assigned the role at the storage account scope.

Should you require further assistance with code samples, Azure CLI equivalents, or troubleshooting, please provide additional details.