# Azure Cosmos DB Workshop

## Deep-Dive Powerpoint Decks

- [Overview, Value Proposition & Use Cases](./decks/Overview-Value-Proposition-Use-Cases.pptx)
- [Resource Model](./decks/Resource-Model.pptx)
- [Request Units & Billing](./decks/Request-Units-Billing.pptx)
- [Data Modeling](./decks/Data-Modeling.pptx)
- [Partitioning](./decks/Partitioning.pptx)
- [SQL API Query](./decks/SQL-API-Query.pptx)
- [Server Side Programming](./decks/Server-Side-Programming.pptx)
- [Troubleshooting](./decks/Troubleshooting.pptx)
- [Concurrency](./decks/Concurrency.pptx)
- [Change Feed](./decks/Change-Feed.pptx)
- [Global Distribution](./decks/Global-Distribution.pptx)
- [Security](./decks/Security.pptx)

## References

- [Use-Case cheat sheet (1-pager)](./decks/1Pager-Use-Cases.pptx)

In addition to the above workshop decks, we have hands-on labs. We have labs available for our .NET sdk and Java sdk below:

#### .NET Lab Prerequisites

Prior to starting these labs, you must have the following operating system and software configured on your local machine:

##### Operating System

- 64-bit Windows 11 Operating System

##### Software

| Software                                    | Download Link                                                |
| ------------------------------------------- | ------------------------------------------------------------ |
| Git                                         | [/git-scm.com/downloads](https://git-scm.com/downloads)      |
| .NET Core 8.0 (or greater) SDK <sup>1</sup> | [dotnet 8](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) |
| Visual Studio Code                          | [/code.visualstudio.com/download](https://code.visualstudio.com/download) |

#### .NET Lab Guides

*It is recommended to complete the labs in the order specified below:*

- [Pre-lab: Creating an Azure Cosmos DB account](dotnet/labs/00-account_setup.md)
- [Lab 1: Creating a container in Azure Cosmos DB](dotnet/labs/01-creating_partitioned_collection.md)
- [Lab 2: Importing Data into Azure Cosmos DB](dotnet/labs/02-load_data.md)
- [Lab 3: Querying in Azure Cosmos DB](dotnet/labs/03-querying_in_azure_cosmosdb.md)
- [Lab 4: Indexing in Azure Cosmos DB](dotnet/labs/04-indexing_in_cosmosdb.md)
- [Lab 5: Building a .NET Console App on Azure Cosmos DB](dotnet/labs/05-build_net_app.md)
- [Lab 6: Multi-Document Transactions in Azure Cosmos DB](dotnet/labs/06-multi-document-transactions.md)
- [Lab 7: Optimistic Concurrency Control in Azure Cosmos DB](dotnet/labs/10-concurrency-control.md)

#### Notes

1. If you already have .NET Core installed on your local machine, you should check the version of your .NET Core installation using the ``dotnet --version`` command.



