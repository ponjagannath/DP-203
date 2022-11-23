# Classify your data

1. Structured
2. semi structured
3. unstructured

# Semi-structured data

1. xml
2. json
3. yaml

# no SQL

1. key value
2. graph
3. Document

# ACID

1. Automicity
2. Consistency
3. Isolation
4. Durability

# OLTP vs. OLAP

## Azure Cosmos DB indexes every field by default

# Azure storage

1. Azure Blobs
2. Azure Files
3. Azure Queues
4. Azure Tables

# Azure storage account settings

1. Subscriptino
2. Location
3. Performance
   - Standard
   - Premium
4. Replication
   - LRS
   - GRS
5. Access Tier
   - Hot
   - Cool
6. Secure Transfer required
   - HTTPS
   - HTTP
7. Virtual networks

# Account kind

1. StorageV2 (general purpose v2)
2. Storage (general purpose v1)
3. Blob storage

## A single Azure subscription can host up to 250 storage accounts per region, each of which has a maximum storage account capacity of 5 PiB.

# Blobs

1. Block blobs - Block blobs are used to hold text or binary files up to ~5 TB (50,000 blocks of 100 MB) in size
2. Page blobs
   - used to hold random-access files up to 8 TB in size
   - random read/write access to 512-byte pages.
3. Append blobs
   - A single append blob can be up to 195 GB.

# Files

- standard Server Message Block (SMB) protocol

# Queues

- Queue messages can be up to 64 KB in size

## Create a storage accout using cli

```bash
az storage account create \
  --resource-group learn-d3f05e4f-3636-44a8-80fc-929fa3c7e3b1 \
  --location westus \
  --sku Standard_LRS \
  --name jagan23112022
```

# REST API endpoint

- https://[name].blob.core.windows.net/
- https://[name].queue.core.windows.net/
- https://[name].table.core.windows.net/
- https://[name].file.core.windows.net/

## Connection strings

```
DefaultEndpointsProtocol=https;AccountName={your-storage};
   AccountKey={your-access-key};
   EndpointSuffix=core.windows.net
```

## Shared access signatures (SAS)

## get connection string using azure cli

```
az storage account show-connection-string \
  --resource-group learn-d3f05e4f-3636-44a8-80fc-929fa3c7e3b1 \
  --query connectionString \
  --name <name>
```

## list the containers in a storage account

```
az storage container list \
--account-name <name>
```

## JS program to create container, uplaod file to a container

```js
#!/usr/bin/env node
require("dotenv").config();

const { BlobServiceClient } = require("@azure/storage-blob");

const storageAccountConnectionString =
  process.env.AZURE_STORAGE_CONNECTION_STRING;
const blobServiceClient = BlobServiceClient.fromConnectionString(
  storageAccountConnectionString
);

async function main() {
  // Create a container (folder) if it does not exist
  const containerName = "photos";
  const containerClient = blobServiceClient.getContainerClient(containerName);
  const containerExists = await containerClient.exists();
  if (!containerExists) {
    const createContainerResponse = await containerClient.createIfNotExists();
    console.log(
      `Create container ${containerName} successfully`,
      createContainerResponse.succeeded
    );
  } else {
    console.log(`Container ${containerName} already exists`);
  }

  // Upload the file
  const filename = "docs-and-friends-selfie-stick.png";
  const blockBlobClient = containerClient.getBlockBlobClient(filename);
  blockBlobClient.uploadFile(filename);

  // Get a list of all the blobs in the container
  let blobs = containerClient.listBlobsFlat();
  let blob = await blobs.next();
  while (!blob.done) {
    console.log(
      `${blob.value.name} --> Created: ${blob.value.properties.createdOn}   Size: ${blob.value.properties.contentLength}`
    );
    blob = await blobs.next();
  }
}
main();
```

# Azure Storage security features

## Encryption at rest

- encrypted by Storage Service Encryption (SSE) with a 256-bit Advanced Encryption Standard (AES) cipher, and is FIPS 140-2 compliant
- virtual machines (VMs) = uses BitLocker for Windows images, and uses dm-crypt for Linux.

## Encryption in transit

- HTTPS
- SMB 3.0

## CORS support

## Role-based access control

## Auditing access

- Storage Analytics logs

# storage account keys

## Storage account keys

# shared access signatures

- service-level SAS
- account-level SAS

# network access to your storage account

## Microsoft Defender for Storage

1. Blob storage
2. Azure Files
3. Azure Data Lake Storage Gen2

# Blob

- virtual directories
