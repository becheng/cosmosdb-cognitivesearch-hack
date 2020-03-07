# cosmosdb-cognitivesearch-hack

Hackathon to store and search through chats/conversations using  CosmosDB + Azure Cognitive Search.
The primary focus will be to store and search through chat content; and not about the development of an actual chat app.  This hack will build upon an existing chat app sample and extend it with search functionality.  

Note: this repo is a fork of [@ealsur's repo](https://github.com/ealsur/serverlessnotifications) which contains a sample chat application built using [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction), [Azure Functions](https://azure.microsoft.com/services/functions/) and [Azure SignalR](https://docs.microsoft.com/azure/azure-signalr/signalr-overview).  For the full description of the how the chat app works, see [@ealsur's sample](https://github.com/ealsur/serverlessnotifications).  

## Objective of the Hack

Development of an MVP to store and search through chat content.

## Target Acceptance Criteria

- Submited chats are saved to CosmosDB.
- A CosmosDB consistency level has been configured that works with Azure Search.
- All saved chats are searchable with low latency, i.e is searable as soon as it's saved.

## Getting started

### Environment setup
Note: I recommend for select the East US 2 region for all components.  At the time of writing this, Azure Signalr Service was not avaiable in all regions during my initial testing.   

1. Create a **Azure Cosmos DB** account to obtain the **Connection String**, the account needs to be a **SQL API account**. REMARKS: the format of the connection string should be "Endpoint=https://{cosmosdb-name}.service.signalr.net;AccessKey={key};".
2. Create a database called **chat** and a collection called **lines** (it can be the smallest possible 400RU collection).  **IMPORTANT**: Due the older version of the SDK, do the following steps to ensure reserved throughput at container level, otherwise the sample will not work.  
    - Uncheck "Provision thoughput" checkbox when creating the chat database.
    - Check "Provision dedicated throughput for this container" and leave the default to 400RUs. 
3. Create a **Azure SignalR** account to obtain the **Connection String** (it can left at the Free tier).  **IMPORTANT**: Select "Serverless" for the Sevice Mode. 
4. Click the Deploy to **Azure button** and it will guide you into automatically creating the Azure Function app with all the code deployed on Azure.

### How to deploy

#### Run it locally

Clone this repo, fill out the [local.settings.json](https://github.com/ealsur/serverlessnotifications/blob/master/src/function/ChangeFeedSignalR/local.settings.json) file with the Connection Strings for Azure Cosmos DB and Azure SignalR and run it with F5!

Open your browser in the base address informed by the Azure Function's runtime (something along the lines of `http://localhost:<some-port>`).

#### Deploy it with one click

Just click in the Deploy to **Azure button** and it will guide you into automatically creating the Azure Function app with all the code deployed on Azure.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fbecheng%2Fcosmosdb-cognitivesearch-hack%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fbecheng%2Fcosmosdb-cognitivesearch-hack%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://armviz.io/visualizebutton.png"/>
</a>

Open your browser in the base address informed by the Azure Function's Portal (something along the lines of `https://<your-app-name>.azurewebsites.net`).

## Architectual (Conceptual)
@TODO: Flesh out the diagram below

![alt text](https://github.com/becheng/cosmosdb-cognitivesearch-hack/blob/master/images/changefeedoverview.png "Conceptual Architecture")

## Points to take into Consideration
- Due to the low latency requiremments, i.e. the search operations must be in sync with population of the CosmosDB, the push model is the only option.  In a push model, the data is programatically sent to the Azure Search index.  Reference: https://docs.microsoft.com/en-us/azure/search/search-what-is-data-import#pushing-data-to-an-index   

## Acknowledges

* The chat app code is based on the [@ealsur's sample](https://github.com/ealsur/serverlessnotifications).
