# DigPacks Document Translator Pro

![MainScreen](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/dda1013f-9e69-44f1-8614-22165d960eb3)


## Introduction
1)  A <b>Power Platform application</b> utilising the Azure AI Translator service, allowing users to upload single or multiple documents and translate them from and to 69 supported languages. 
2) Files are <b>uploaded via application from the user’s local machine or any other file source</b> which in turn deploys them as blobs into Azure Blob Storage account, passes them through the Translator service, and returns the translated document into Blob, ready for the user to view. 
3) This solution <b>preserves the presentation of the source file</b> (so, doesn’t just extract text and translate that like AI Builder services; rather, translates the text and retains the overall structure of the source document, and outputs in the same file format as the source document).
4) The <b>source document’s language is automatically detected</b>; meaning that users don’t need to know what language the source document is in before translating it to, most likely, English. 
5) Translate <b>large files in large batches</b> – individual document size up to 40MB, up to 1000 documents in one translation batch (max 250MB per batch).

## Account permissions

<b>IMPORTANT:</b>

In order to deploy and run this service, you'll need:

### Azure account permissions

- Azure account: If you're new to Azure, [get an Azure account for free](https://azure.microsoft.com/en-us/free/) and you'll get some free Azure credits to get started.
- An Azure subscription.
- Azure account permissions:
  - Your account must have ``` Microsoft.Authorization/roleAssignments/write ``` permissions, such as [RBAC Administrator](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#role-based-access-control-administrator-preview), [User Access Administrator](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator) or [Owner](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#owner). If you don't have subscription-level permissions, you must be granted [RBAC](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#role-based-access-control-administrator-preview) for an existing resource group and deploy to that existing group.
  - Your Azure account also needs ``` Microsoft.Resources/deployments/write ``` permissions on the subscription level.
  - If you are not sure if you have the correct permissions, ask your Global Administrator to undertake the Azure deployment.

### Power Platform account permissions
- Power Apps and Power Automate Premium licenses.
- The System Administrator role on your target environment(s). 

## Services used
- [Power Apps](https://www.microsoft.com/en-us/power-platform/products/power-apps)
- [Power Automate](https://powerautomate.microsoft.com/en-gb/)
- [Azure AI Translator](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/translator/)
- [Azure Blob Storage Account](https://azure.microsoft.com/en-gb/products/storage/blobs/)

## Service configuration
### 1. Azure Blob Storage Account
- Standard performance tier* - StorageV2.
- Hot access tier.
- Redundancy default to LRS, but can choose from LRS, GRS, RA-GRS, ZRS, GZRS or RZ-GZRS.
- Region default to UK South, but can choose from UK South or UK West.
- TLS 1.2
- HTTPS traffic only
- Blob access default to public.
- Shared key access allowed.
- Public network access enabled.
- Soft delete retention for Blob, Container and Share = 7 days.
- Infrastructure encryption is enabled.
- Microsoft Managed Keys (MMK) only.

*Premium performance tier not currently supported. Coming soon. 

### 2. Azure AI Translator (part of Azure AI* suite)
- Region - UK South by default, but can choose from UK South or West Europe.
- Pricing tier - Standard S1 (PAYG) by default, but can choose from S1, S2, S3, S4, C2, C3, C4 or D3.
- Networking - public.
- System-assigned managed identity to communicate with the Storage Account.

*Previously known an Azure Cognitive Services.

### 3. Power Apps
- [Power Apps Premium](https://powerapps.microsoft.com/en-us/pricing/) licenses for all users.

### 4. Power Automate
- [Power Automate Premium](https://powerautomate.microsoft.com/en-us/pricing/) licenses for all users.

## Installation / deployment

### Cost estimation
Pricing varies dependant on region and consumption, so it is not possible to predict exact costs for your usage; but you can use the [Azure pricing calculator](https://azure.com/e/8ffbe5b1919c4c72aed89b022294df76) for the resources used.

Note: You cannot use the Free tier of the Azure AI Translator service as this does not support document translation. 

:boom: You should not incur costs if the solution is not used, but delete the resources and redeploy them if you like to avoid charges. You may incur charges if you do not use the solution but deploy, for example but not limited to, a higher tier than S1 for the Translator service.

### Enabling authentication
By default, the only people who can use the solution are those whom you share the Power App with; however, the Azure AI Translator service and Storage Account will have no authentication or access restrictions enabled, meaning anyone who may have access to the endpoint(s) and key(s) in Azure can utilise the services over API. You can secure the services further if you wish by deploying a Virtual Network, using RBAC or another authentication method.

### Enabling Application Insights
You can deploy Application Insights to monitor the usage of both your Storage Account and Azure AI Translator service if you wish. Application Insights allows for the tracing of each request along with logging of any errors. 

### Adding users

The recommended method of sharing the solution with other internal users, as per the license, is to [add them to a new or existing Azure Security Group](https://learn.microsoft.com/en-us/entra/fundamentals/how-to-manage-groups). This can either be direct or dynamic membership, but must be an Azure Security Group (not an M365 Group). 

When the Power Platform solution has been deployed to your Production environment, [share the Power App](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/share-app) with the Azure Security Group. Assign the group the solution's Security Role of "DigPacks Translator Pro - User". 






