# DigPacks Translator Pro

## Introduction
1)  A Power Platform application utilising Microsoft Translator service via Managed Identity, allowing users to upload single or multiple documents and translate them from and to 69 supported languages. 
2) Files are uploaded via application from the user’s local machine, OneDrive, or SharePoint which in turn deploys them as blobs into Azure Blob Storage account, passes them through the Translator service, and returns the translated document into Blob, ready for the user to view. 
3) This solution preserves the presentation of the source file (so, doesn’t just extract text and translate that like AI Builder services; rather, translates the text and retains the overall structure of the source document, and outputs in the same file format as the source document).
4) The source document’s language is automatically detected; meaning that users don’t need to know what language the source document is in before translating it to, most likely, English. 
5) Translate large files in large batches – individual document size up to 40MB, up to 1000 documents in one translation batch (max 250MB per batch).

#### Services used
- Power Apps
- Power Automate
- Microsoft Translator
- Azure Storage Account

#### Installation

a. 






