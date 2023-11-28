# DigPacks Document Translator Pro

![MainScreen](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/dda1013f-9e69-44f1-8614-22165d960eb3)


## Introduction
1)  A <b>Power Platform application</b> utilising the Azure AI Translator service, allowing users to upload single or multiple documents and translate them from and to 69 supported languages. 
2) Files are <b>uploaded via application from the user’s local machine or any other file source</b> which in turn deploys them as blobs into Azure Blob Storage account, passes them through the Translator service, and returns the translated document into Blob, ready for the user to view. 
3) This solution <b>preserves the presentation of the source file</b> (so, doesn’t just extract text and translate that like AI Builder services; rather, translates the text and retains the overall structure of the source document, and outputs in the same file format as the source document).
4) The <b>source document’s language is automatically detected</b>; meaning that users don’t need to know what language the source document is in before translating it to, most likely, English. 
5) Translate <b>large files in large batches</b> – individual document size up to 40MB, up to 1000 documents in one translation batch (max 250MB per batch).
6) <b>Transcribe and translate speech</b> all within the app. Apply punctuation and breaks where you like, and the translator does the rest.
7) A <b>custom Security Role so that only people/groups you choose can use the application</b>.

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
- [Azure AI Speech](https://azure.microsoft.com/en-us/products/ai-services/ai-speech)
- [Azure Blob Storage Account](https://azure.microsoft.com/en-gb/products/storage/blobs/)

![infraMap](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/d4895bdc-9040-4930-a948-167374658d88)

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

### 3. Azure AI Speech (part of Azure AI* suite)

- Region - UK South by default, but can choose from UK South or West Europe.
- Pricing tier - S0 Standard.
- Networking - public
- System-assigned managed identity to communicate with the Storage Account.

*Previously known an Azure Cognitive Services.

### 4. Power Apps
- [Power Apps Premium](https://powerapps.microsoft.com/en-us/pricing/) licenses for all users.

### 5. Power Automate
- [Power Automate Premium](https://powerautomate.microsoft.com/en-us/pricing/) licenses for all users.

## Installation / deployment

### Cost estimation
Pricing varies dependant on region and consumption, so it is not possible to predict exact costs for your usage; but you can use the [Azure pricing calculator](https://azure.com/e/8ffbe5b1919c4c72aed89b022294df76) for the resources used.

Note: You cannot use the Free tier of the Azure AI Translator service as this does not support document translation. 

:boom: You should not incur costs if the solution is not used, but delete the resources and redeploy them if you like to avoid charges. You may incur charges if you do not use the solution but deploy, for example but not limited to, a higher tier than S1 for the Translator service. :boom:

### Enabling authentication
By default, the only people who can use the solution are those whom you share the Power App with; however, the Azure AI Translator service and Storage Account will have no authentication or access restrictions enabled, meaning anyone who may have access to the endpoint(s) and key(s) in Azure can utilise the services over API. You can secure the services further if you wish by deploying a Virtual Network, using RBAC or another authentication method.

### Enabling Application Insights
You can deploy Application Insights to monitor the usage of both your Storage Account and Azure AI Translator service if you wish. Application Insights allows for the tracing of each request along with logging of any errors. 

### Adding users

The recommended method of sharing the solution with other internal users, as per the license, is to [add them to a new or existing Azure Security Group](https://learn.microsoft.com/en-us/entra/fundamentals/how-to-manage-groups). This can either be direct or dynamic membership, but must be an Azure Security Group (not an M365 Group). The Security Group must have the permanent assignment of Storage Blob Data Contributor to the Storage Account deployed. 

When the Power Platform solution has been deployed to your Production environment, [share the Power App](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/share-app) with the Azure Security Group. Assign the group the solution's Security Role of "DigPacks Translator Pro - User".

**IMPORTANT: To get you set up quickly, we use the API Key authentication method to access the services via the Power Platform. However, when you deploy, feel free to change this to Azure Entra ID authentication if you like.**

## Azure infrastructure deployment

The deployment of the infrastructure for this solution is undertaken via ARM templates as IaC ('infrastructure as code'). IaC streamlines and automates the provisioning of infrastructure, ensuring consistent, repeatable setups. It enables quick scaling, version-controlled changes, and significantly reduces manual errors and operational costs. IaC fosters collaboration with code that's easily shared and maintained, offering swift recovery and environment parity. It's a cornerstone of DevOps, promoting agility and transparency across development, staging, and production environments.

There are many ways by which you can deploy the infrastructure for this solution, but the supported method is to use the ARM (Azure Resource Manager) templates provided in this repo. You can customise them as you see fit, but they provide the baseline for what is needed for the application to operate.

### Azure template installation (quickstart)

1. Download template.json from this repo.
2. In Azure Portal, search for and select 'Template specs'. Alternatively, you can search for 'Deploy a custom template'.
3. Upload the template.json file from this repo.
4. Follow the on-screen prompts to complete the deployment. You must make some choices on the sku and pricing tiers you want.
5. When the services have deployed, you will need to make a note of the services' endpoints and access keys.
6. Navigate to the resource group you just deployed and, for each service (Azure AI Translator, Azure AI Speech service and the Storage Account), note the endpoint URL and one of the access keys.

### Manually deploy each Azure service

This can be done via IaC, but to keep things simple, we'll use the Azure Portal to get you up and running. 

#### Create a new resource group

1. In a Subscription, create a new resource group for where the resources will live.
2. In the newly created resource group, click "+ Create" from the taskbar.

#### Deploy the Blob storage account

3. Search for "storage account" and select the one published by Microsoft. Click "Create" > "Storage account".
4. Give the storage account a name, choose the region, performance and redundancy options to suit you.
5. In the Advanced tab, ensure that "Allow enabling anonymous access on individual containers" is checked.
6. Click through to "Review" and click "Create". Your storage account is now deployed.
7. Go to the Storage Account resource. Click the "Containers" blade on the left-hand menu.
8. Create 5 containers, called "audio-to-translate", "files-to-translate", "source-history", "transcribed-audio", "translated-files". For each container, ensure that the access level is set to "Anonymous access level" > "Container".
9. Click on one of the Containers created and select the "Properties" blade from the left-hand side. Copy the URL displayed up to the "/" before the container name. Paste it into Notepad temporarily.
10. Step back into the Storage Account main page and click the "Access keys" blade on the left-hand side. Copy one of the keys and paste it into Notepad temporarily. Also make a note of the 'Storage account name'.
11. Your storage account is ready.

#### Deploy the Azure AI Translator service

1. From the resource group you created earlier, search for "translator" and select the Translator service. Click "Create" > "Translator".
2. Choose the Region for your Translator service. **This must match the Region chosen for your Storage Account and MUST NOT be the Global region**
3. Give your Translator service a name.
4. Select the pricing tier. Do not select the Free tier, as this does not support document translation.
5. In the Identity tab, make sure "System assigned managed identity" is set to "On".
6. Move forwards to Review + Create to start the deployment.
7. Once the Translator service is deployed, go to it and select the "Identity" blade from the left-hand panel. System assigned > Status should be "On".
8. Under Permissions, click "Azure role assignments".
9. Select "+ Add role assignment".
10. In the pop up window, select Scope > Storage.
11. Ensure the correct subscription is selected.
12. Under Resource, choose the Storage Account you deployed earlier.
13. Under Role, select "Storage Blob Data Contributor" and click Save.
14. Select the "Keys and Endpoint" blade on the left-hand panel, copy and note one of the keys. Make a note of both the Text Translation and Document Translation endpoints under Web API.
15. Your translator service is ready.

#### Deploy the Azure AI Speech service

1. From the resource group you created earlier, search for "speech" and select the Speech service. Click "Create" > "Speech".
2. Choose the Region for your Speech service. **This must match the Region chosen for your Storage Account and MUST NOT be the Global region**
3. Give your Speech service a name.
4. Select the "Standard S0" pricing tier.
5. In the Identity tab, make sure "System assigned managed identity" is set to "On".
6. Move forwards to Review + Create to start the deployment.
7. Once the Speech service is deployed, go to it and select the "Identity" blade from the left-hand panel. System assigned > Status should be "On".
8. Under Permissions, click "Azure role assignments".
9. Select "+ Add role assignment".
10. In the pop up window, select Scope > Storage.
11. Ensure the correct subscription is selected.
12. Under Resource, choose the Storage Account you deployed earlier.
13. Under Role, select "Storage Blob Data Contributor" and click Save.
14. Select the "Keys and Endpoint" blade on the left-hand panel, copy and note one of the keys. Make a note of both Endpoint given.
15. Your speech service is ready.

## Power Platform deployment

After you have deployed the necessary infrastructure and noted the keys and endpoints as per the Infrastructure deployment section, you can import the .zip file containing the Power App, Power Automate flows and other elements. 

1. Go to the Power Platform environment where you want to deploy Translator Pro.

2. Select import a solution
![Deploy1](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/abd97b97-fba7-4952-8712-e5ee323e8f01)

3. Click browse and Select the .zip solution
![Deploy2](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/f4c4846f-3525-40ff-bbbd-6a296f6b148c)
![Deploy3](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/4dd37ffb-7879-48ce-8c1f-bee52256ca5a)

4. Click 'Next'
![Deploy4](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/c3b2e2ef-7b41-4e20-8ce2-056edf34e451)

5. Click 'Next'
![Deploy5](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/dfdc0c8b-9d1b-4d22-87c9-69c566be4f0e)

6. Here you need to set the connections for each service which the App uses.
![Deploy6](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/b638a776-17ee-47e6-bc72-5dca560f5655)

7. For Microsoft Translator, paste in the 'Translator resource name' and 'Resource Key'.
![Deploy7](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/c4377f8e-3ee4-40a0-8776-c0d596f2a130)

8. Click 'Create'.
![Deploy8](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/e5800a01-2fba-4950-8e78-91505d0cb9a8)

9. In "Apply changes", click Refresh.
![Deploy9](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/5d1dc584-457c-47a1-92d3-a549a21c8cae)

10. For Azure Blob Storage, choose type of 'Access Key', paste the 'Azure Storage account name or blob endpoint' and 'Azure Storage Account Access Key'.
![Deploy10](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/9a0f86f2-701d-4010-b7c1-8a2bf184dbce)
![Deploy11](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/dc2cd103-0569-4e1e-9d15-7faa5ee48a5a)

11. For Azure Batch Speech-to-text, choose 'Api Key', paste the 'Account Key' and enter the Region. For UK South, this should be entered as "uksouth".
![Deploy12](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/9eb535bd-a6fc-407d-91ee-75800fdabe3b)

12. When you have set all the connections, you should now click 'Import'.
![Deploy6](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/dfdb863d-fd71-47b7-8542-0618e76601a4)

13. You will now see the banner of "Currently importing solution "Digpacks Translator"".
![Deploy13](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/ec5fc3f6-fec6-4edb-8693-68323d61371c)

14. After a moment, the import will complete and the banner will show "Solution "Digpacks Translator imported successfully".
![Deploy14](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/4f3d9ed4-2236-4409-9f33-717e4caa0aad)

15. In your solution, select the Environment Variable. Change it to the endpoint for your Blob Storage account - it will look like this:

``` https://<account name>.blob.core.windows.net ```

16. Navigate to Apps > click on the app and click Play

17. On first launch, you will be asked to confirm your authorisation to the connected services. Click 'Allow'.
![Deploy15](https://github.com/DigPacksPhilMVP/digpacks-translator-pro/assets/147208426/6435d8ee-bc72-4042-af44-b0cd4a7d1f2f)

18. The app is now deployed. Feel free to share it with users/groups, using the in-built Security Role of "Digpacks Translator Pro - User" which contains everything they need to use the app and services. You will need to share the Translator key with them when they first load the app.





