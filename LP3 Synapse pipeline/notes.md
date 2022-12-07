# Auzre pipelines

## ETL

## ELT

## ELTL

# ADF supports 2 types of data integration patterns

1. Modern Data Warehouse workload
2. Advanced Analytical Workload

# 4 steps of ADF pipelines

1. Collect and Connect
2. Transform and Enrich
3. Publish
4. Monitor

# 4 Core Components of ADF

1. Linked services - like connection string
2. Dataset - named view of data
3. Activities - action on the data
4. Pipeline

## Control flow

## Parameters

## Integration Runtimes

1. Azure
2. Self Hosted
3. Azure - SSIS

# ADF security

1. to create ADF child resource from Portal - you must belong to the Data Factory Contributor Role at RG or above.
2. to craete with Power Shell or SDK , contributor Role at RG or above is enough.

## Data Factory contributor role

```
{
    "name": "<Name of the linked service>",
    "properties": {
        "type": "<Type of the linked service>",
        "typeProperties": {
              "<data store or compute-specific type properties>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## Linked service example - Azure SQL Database

```
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<server-name>.database.windows.net,1433;Database=ctosqldb;User ID=ctesta-oneill;Password=P@ssw0rd;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```

```
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=ctostorageaccount;AccountKey=<account-key>"
    }
  }
}
```

## Create Dataset

```
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "linkedServiceName": {
                "referenceName": "<name of linked service>",
                "type": "LinkedServiceReference",
        },
        "schema": [
            {
                "name": "<Name of the column>",
                "type": "<Name of the type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        }
    }
}
```

# Example

```
  {
      "name": "InputDataset",
      "properties": {
          "linkedServiceName": {
              "referenceName": "AzureStorageLinkedService",
              "type": "LinkedServiceReference"
          },
          "annotations": [],
          "type": "Binary",
          "typeProperties": {
              "location": {
                  "type": "AzureBlobStorageLocation",
                  "fileName": "emp.txt",
                  "folderPath": "input",
                  "container": "adftutorial"
              }
          }
      }
  }
```

# 3 type of activities

1. Data movement activity
2. Data transformation activity
3. Control activity

# Control activities

- Execute Pipeline Activity
- ForEachActivity
- WebActivity
- Lookup Activity
- Get Metadata Activity
- Until Activity
- If Condition Activity
- Wait Activity

## defining execution (compute )activities

```
{
    "name": "Execution Activity Name",
    "description": "description",
    "type": "<ActivityType>",
    "typeProperties":
    {
    },
    "linkedServiceName": "MyLinkedService",
    "policy":
    {
    },
    "dependsOn":
    {
    }
}
```

## Defining control activity

```
{
    "name": "Execution Activity Name",
    "description": "description",
    "type": "<ActivityType>",
    "typeProperties":
    {
    },
    "linkedServiceName": "MyLinkedService",
    "policy":
    {
    },
    "dependsOn":
    {
    }
}
```

## Defining pipeline

```
{
    "name": "PipelineName",
    "properties":
    {
        "description": "pipeline description",
        "activities":
        [
        ],
        "parameters": {
         }
    }
}
```

## pipeline example

```
{
    "name": "MyFirstPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@ctostorageaccount.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@ctostorageaccount.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
              },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2017-04-01T00:00:00Z",
        "end": "2017-04-02T00:00:00Z",
        "isPaused": false,
        "hubName": "ctogetstarteddf_hub",
        "pipelineMode": "Scheduled"
    }
}
```

# Integration runtimes

- An integration runtime provides the infrastructure for the activity and linked services

## Data integration capability of Integratoin runtime

1. Data flow
2. Data movement
3. Activity Dispatch
4. SSIS package execution

## Auto-resolve

## Integration runtime types - connectVia

1. Azure
2. Self-hosted
3. Azure SSIS

# Petabyte scale ingestin with ADF

## Copy activity - support of over 100 connectors

## Ingesting data using compute resource

1. HDInsights
2. Azure Batch
3. Azure ML studio Machine
4. Azure ML
5. Azure Data lake analytics
6. Azure SQL
7. Databricks
8. Azure Function

# ADF connectors

## Support file formats includes

- Avro
- Binary
- Delimited
- JSON
- ORC
- Parquet

## Data source example

- Azure
- Databases
- NoSQL store
- File
- Generic protocols
- Services and Apps

# Self-hosted integration runtime

- used when a private or virtual network is involved

## Manage -> integration runtime

## Powershell to create self hosted integrated runtime

```
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName -Type SelfHosted -Description "selfhosted IR description"
```

# Get the key for the self hosted integrated runtime

```
Get-AzDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName
```

# Azure integration runtime

## powershell to create the Azure integrated run time

```
Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName "SampleV2DataFactory1" -Name "MySampleAzureIR" -ResourceGroupName "ADFV2SampleRG" -Type Managed -Location "West Europe"
```

# security considerations

- Use virtual networks to secure Azure resources
- Use services to detect and prevent intrusions
- Simplify the management of security rules using network service tags

# Identity and access control

- Administrative accounts
- Use Active Directory to make use of single sign-on

# Data protection

- Use Role Based Access Control (RBAC) to control access to the resources
-

# Logging and monitoring

- Configure central security log management
- Monitor and log the configuration and network packet traffic of virtual networks, subnets, and NICs
- Enable audit logging

# next: Perform code-free transformation at scale with Azure Data Factory or Azure Synapse Pipeline

# Mapping Data flow

## Mapping Data flow activities runs on underlying Saprk clusters

## Transformation using compute resources

1. On-demand HDInsight cluster or your own HDInsight cluster
2. Azure Batch
3. Azure Machine Learning Studio Machine
4. Azure Data Lake Analytics
5. Azure SQL, Azure SQL Data Warehouse, SQL Server
6. Azure Databricks
7. Azure Function

## Transforming data using SQL Server Integration Services (SSIS) packages

# 3 ADF transformation catagories

1. Schema modifier
2. Row modifier
3. Multiple inputs/outputs

# List of ADF transformation

1. Aggregate
2. Alter row
3. Conditional Split
4. Derived Columns
5. Exists
6. Filter
7. Flatten
8. Join
9. Lookup
10. New Branch
11. Pivot
12. Select
13. Sink
14. Sort
15. Source
16. Surrogate key
17. Union
18. UnPivot
19. Window

# Data Flow Expression Builder

# Data Flow source can use the following sources

- Azure Blob Storage (JSON, Avro, Text, Parquet)
- Azure Data Lake Storage Gen1 (JSON, Avro, Text, Parquet)
- Azure Data Lake Storage Gen2 (JSON, Avro, Text, Parquet)
- Azure Synapse Analytics
- Azure SQL Database
- Azure Cosmos DB

# Power query feature

## power query supports the following data sources

- Azure blob storage
- ADLS Gen1
- ADLS Gen2
- Azure SQL Database
- Azure Synapse Analytics

# Compute transformations with ADF

## Data bricks with ADF

- Access token

## SSIS with ADF

- SSIS packages can be lifted and shifted
- Azure-SSIS integration runtime
- pre requiste - SSIS Catalog (SSISDB) deployed on a SQL Server SSIS instance

# slowly changing dimension (SCD)

## Type 1 SCD

## Type 2 SCD - Supports versioning

## Type 3 SCD

## Type 6 SCD

# Module: Orchestrate data movement and transformation in Azure Data Factory or Azure Synapse Pipeline

# Control flow

1. Chanining Activities - dependsOn
2. Branching activities - if else

## parameters

## Custom state passing - you can access the output of the previous activity

## Looping containers - do while

## Trigger-based flows

## Invoke a pipeline from another pipeline

## Delta flows

## Other control flows

1. Web activity
2. Get metadata activity

## 3 catagories of activities

1. Data movement activities
2. Data transformation activities - Data Flow, Azure Function, Spark, and others
3. Control activities - 'get metadata', 'For Each', and 'Execute Pipeline

## Activity Dependency

### 4 dependency condition

1. Succeeded
2. Failed
3. Skipped
4. Completed

### If you have multiple activities in a pipeline and subsequent activities are not dependent on previous activities, the activities may run in parallel.

# Debug

### there is no need to publish changes in the pipeline or activities before you want to debug

### To debug a part of the pipeline, you can set a breakpoint ( Debug Until )

## Publish all - This action publishes entities (datasets, and pipelines) you created to Data Factory.

## Data Flow Debug

### If you have parameters in your Data Flow or any of its referenced datasets, you can specify what values to use during debugging by selecting the Parameters tab

### During debugging, sinks are not required and are ignored in the dataflow

## sandbox environment ?

## Monitor debug runs - debug run history for 15 days

# Module: Add parameters to data factory components

[Module link](https://learn.microsoft.com/en-us/training/modules/orchestrate-data-movement-transformation-azure-data-factory/5-add-parameters-components)

## Parameterize linked services in ADF

### ex) create a parametrized linked service for databases in an Azure SQL server.

## Global parameters in ADF

### A use-case for setting global parameters is when you have multiple pipelines where the parameters names and values are identical.

## Use global parameters in a pipeline

- pipeline().globalParameters

## Global parameters in CI/CD

1. Include global parameters in the Azure Resource Manager template
2. Deploy global parameters via a PowerShell script

## Parameterize mapping dataflows

## Create parameters in dataflow

## Assign parameters from a pipeline in mapping dataflow

# Unit: Exercise - Integrate a Notebook within Azure Synapse Pipelines

# Unit: Execute data factory packages

# Module: Execute existing SSIS packages in Azure Data Factory or Azure Synapse Pipeline

[Module link](https://learn.microsoft.com/en-us/training/modules/execute-existing-ssis-packages-azure-data-factory/)

# SQL Server integration services

## A SSIS solution usually consists of one or more SSIS projects, each containing one or more SSIS packages.

## SSIS projects

- a project is the unit of deployment for SSIS solutions

## SSIS packages

- A project contains one or more packages, each defining a workflow of tasks to be executed

## Azure SSIS Integration Runtime

- An Integration Runtime (IR) provides the bridge between the Activity and Linked Services.
- SSIS IR, should be the same as the location of your own Azure SQL Database or Azure SQL Database managed instance server where SSISDB is to be hosted

# Module: Operationalize your Azure Data Factory or Azure Synapse Pipeline

[Module link](https://learn.microsoft.com/en-us/training/modules/operationalize-azure-data-factory-pipelines/)

## language support in Azure Data Factory

```py
pip install azure-mgmt-datafactory
```

Create ADF using python

```py
from azure.common.credentials import ServicePrincipalCredentials
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.datafactory import DataFactoryManagementClient
from azure.mgmt.datafactory.models import *
import time

#Create a data factory
subscription_id = '<Specify your Azure Subscription ID>'
credentials = ServicePrincipalCredentials(client_id='<Active Directory application/client ID>', secret='<client secret>', tenant='<Active Directory tenant ID>')
adf_client = DataFactoryManagementClient(credentials, subscription_id)

rg_params = {'location':'eastus'}
df_params = {'location':'eastus'}

df_resource = Factory(location='eastus')
df = adf_client.factories.create_or_update(rg_name, df_name, df_resource)
print_item(df)
while df.provisioning_state != 'Succeeded':
    df = adf_client.factories.get(rg_name, df_name)
    time.sleep(1)
```

### supported languages

- .NET
- REST APIs
- PowerShell
- Azure Resource Manager Templates
- Data flow scripts - Data flow script (DFS) is the underlying metadata , that is used to execute the transformations that are included in a mapping data flow

### source control for ADF

- Azure Repos or GitHub
- Changes made via PowerShell or an SDK are published directly to the Data Factory service, and are not entered into Git.

### Advantages of Git integration

- Source control
- Partial saves
- Collaboration and control
- Better CI/CD
- Better Performance - 10 times faster via git than data factory service

### 3 ways to connect to git

1. Home page
2. Authoring canvas
3. Management hub

## Creating feature branches

- You are only allowed to publish to the Data Factory service from your collaboration branch ( main branch)

## Configure publishing settings

```json
{
  "publishBranch": "factory/adf_publish"
}
```

- After you have merged changes to the collaboration branch , click Publish to manually publish your code changes in the collaboration branch to the Data Factory service.

## Best practices for Git integration

### Permissions - to be able to publish to ADF -> Data Factory contributor access

### Using passwords from Azure Key Vault

### Switch to a different Git repository

## Unit: Continuous integration and deployment

[unit link](https://learn.microsoft.com/en-us/training/modules/operationalize-azure-data-factory-pipelines/4-continuous-integration-deployment)

- continuous integration and delivery (CI/CD) means moving Data Factory pipelines from one environment (development, test, production) to another
- Azure Data Factory utilizes Azure Resource Manager templates to store the configuration of your various Azure Data Factory entities (pipelines, datasets, data flows, and so on).

- Only the development factory is associated with a git repository. The test and production factories shouldn't have a git repository associated with them and should only be updated via an Azure DevOps pipeline or via a Resource Management template.

### custom parameter file name - arm-template-parameters-definition.json

## Hotfix production environment

## Best practices for Continuous Integration/Continuous Delivery

# Monitor Azure Data Factory pipelines

## Monitor using Azure Monitor

- Data Factory stores pipeline-run data for only 45 days.
- Use Azure Monitor if you want to keep that data for a longer time.

# Set up alerts for Azure Data Factory

# Rerun Azure Data Factory pipelines
