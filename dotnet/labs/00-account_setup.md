# Account Setup

In this lab, you will setup your Azure subscription with the required resources needed to perform the Cosmos DB labs. The estimated cost to run these labs if you do it in one sitting is ~$100 USD.

## Prerequisites

- Azure Paid Subscription
- [Azure PowerShell Module](https://docs.microsoft.com/en-us/powershell/azure/install-az-ps)

## Lab Content Setup

1. To begin setup, Git clone or download the repo containing these instructions from [Github](https://github.com/AzureCosmosDB/labs).

2. Open Windows Powershell
3. Navigate to the folder containing your downloaded copy of the repo
4. Inside the repo, navigate to the **dotnet\setup** folder:

   ```powershell
   cd .\dotnet\setup\
   ```

5. To enable the setup scripts to run in your current Powershell session, enter the following:

   ```powershell
   Set-ExecutionPolicy Unrestricted -Scope Process

   > This setting will only apply within your current Powershell window.

6. Many of the labs refer to pre-built code to use as a starting point for the lab instructions. To automatically copy this starter code for the labs into a **CosmosLabs** folder in your **Documents** folder run the labCodeSetup.ps1 script:

   ```powershell
   .\labCodeSetup.ps1
   ```

   > The starter code for each lab is located inside the **templates** folder. To use a folder other that **Documents\CosmosLabs** for your lab code, the files can be copied manually instead.

7. Now using Azure CLI in the Azure Portal Shell do those commands:

If you don't have already a CosmosDB account and a resource group run those following commands:

```bash
RESOURCE_GROUP=cosmoslabs
LOCATION=canadacentral
COSMOS_DB_NAME=cosmos-$(uuidgen | tr -d '-')

az group create -n $RESOURCE_GROUP -l $LOCATION
az cosmosdb create -n $COSMOS_DB_NAME -g $RESOURCE_GROUP --locations regionName=$LOCATION failoverPriority=0 
```

```bash
az cosmosdb sql database create -a $COSMOS_DB_NAME -g $RESOURCE_GROUP -n 'NutritionDatabase' --throughput 1000
az cosmosdb sql database create -a $COSMOS_DB_NAME -g $RESOURCE_GROUP -n 'StoreDatabase' --throughput 1000
az cosmosdb sql database create -a $COSMOS_DB_NAME -g $RESOURCE_GROUP -n 'FinancialDatabase' --throughput 1000
```

```bash
az cosmosdb sql container create -a $COSMOS_DB_NAME -g $RESOURCE_GROUP -d 'StoreDatabase' -n 'CartContainer' --partition-key-path '/Item'
az cosmosdb sql container create -a $COSMOS_DB_NAME -g $RESOURCE_GROUP -d 'StoreDatabase' -n 'CartContainerByState' --partition-key-path '/BuyerState'
az cosmosdb sql container create -a $COSMOS_DB_NAME -g $RESOURCE_GROUP -d 'StoreDatabase' -n 'StateSales' --partition-key-path '/State' 
```

```bash
az cosmosdb sql container create -a $COSMOS_DB_NAME -g $RESOURCE_GROUP -d 'NutritionDatabase' -n 'FoodCollection' --partition-key-path '/foodGroup'
```

```bash

az cosmosdb sql container create -a $COSMOS_DB_NAME -g $RESOURCE_GROUP -d 'FinancialDatabase' -n 'PeopleCollection' --partition-key-path '/accountHolder/LastName'

az cosmosdb sql container create -a $COSMOS_DB_NAME -g $RESOURCE_GROUP -d 'FinancialDatabase' -n 'TransactionCollection' --partition-key-path '/costCenter'
```