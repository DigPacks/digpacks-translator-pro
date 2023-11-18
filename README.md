# DigPacks Translator Pro

## Introduction
1)  A <b>Power Platform application</b> utilising the Azure Microsoft Translator service, allowing users to upload single or multiple documents and translate them from and to 69 supported languages. 
2) Files are <b>uploaded via application from the user’s local machine or any other file source</b> which in turn deploys them as blobs into Azure Blob Storage account, passes them through the Translator service, and returns the translated document into Blob, ready for the user to view. 
3) This solution <b>preserves the presentation of the source file</b> (so, doesn’t just extract text and translate that like AI Builder services; rather, translates the text and retains the overall structure of the source document, and outputs in the same file format as the source document).
4) The <b>source document’s language is automatically detected</b>; meaning that users don’t need to know what language the source document is in before translating it to, most likely, English. 
5) Translate <b>large files in large batches</b> – individual document size up to 40MB, up to 1000 documents in one translation batch (max 250MB per batch).

## Azure account permissions

<b>IMPORTANT:</b>

In order to deploy and run this service, you'll need:

- Azure account: If you're new to Azure, [get an Azure account for free](https://azure.microsoft.com/en-us/free/) and you'll get some free Azure credits to get started.
- An Azure subscription.
- Azure account permissions:
  - Your account must have ``` Microsoft.Authorization/roleAssignments/write ``` permissions, such as [RBAC Administrator](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#role-based-access-control-administrator-preview), [User Access Administrator](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator) or [Owner](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#owner). If you don't have subscription-level permissions, you must be granted [RBAC](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#role-based-access-control-administrator-preview) for an existing resource group and deploy to that existing group.
  - Your Azure account also needs ``` Microsoft.Resources/deployments/write ``` permissions on the subscription level.
  - If you are not sure if you have the correct permissions, ask your Global Administrator

## Services used
- Power Apps
- Power Automate
- Microsoft Translator
- Azure Blob Storage Account

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

* Premium performance tier not currently supported. Coming soon. 

### 2. Microsoft Translator (part of Cognitive Services suite)
- Region - UK South default, but can choose from UK South or West Europe.
- Pricing tier - Standard S1 (PAYG) by default, but can choose from S1, S2, S3, S4, C2, C3, C4 or D3.
- Networking - public.
- System-assigned managed identity to communicate with the Storage Account.

### 3. Power Apps
- Power Apps Premium licensing for all users.

### 4. Power Automate
- Power Automate Premium licensing for all users.

## Installation / deployment

### Cost estimation
Pricing varies dependant on region and consumption, so it is not possible to predict exact costs for your usage; but you can use the [Azure pricing calculator](https://azure.com/e/8ffbe5b1919c4c72aed89b022294df76) for the resources used.

You cannot use the Free tier of Translator service as this does not support Document translation. 

:boom: You should not incur costs if the solution is not used, but delete the resources and redeploy them if you like to avoid charges. You may incur charges if you do not use the solution but deploy, for example but not limited to, a higher tier than S1 for the Translator service.






