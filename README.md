# Application-Insights-to-PowerBI-with-ADX

## 1.0 Solution Summary
### 1.1 Overview
This solution was architected to provide application telemetry reporting through PowerBI utilizing Azure Application Insights and Azure Data Explorer.  Application logs are pushed to Azure Data Explorer through an Event Hub utilizing diagnostic settings.  Then PowerBI is connected to ADX utilizing Custom M Query Parameters and direct query.

This solution should be considered in cases where log files are too large to ingest into PowerBI via "import" mode.

### 1.2 Prerequisites
To implement this solution you will need access to an Azure environment with the ability to create resources.  Set up a free azure account [here](https://azure.microsoft.com/en-ca/free/search/?OCID=AIDcmmqz3gd78m_SEM_af052705cd091cba049645f7a218ed35:G:s&ef_id=af052705cd091cba049645f7a218ed35:G:s&msclkid=af052705cd091cba049645f7a218ed35)


This solution requires an application to generate events which are sent to Application Insights.  For sample applications that can generate events please visit the [Azure-Samples github](https://github.com/Azure-Samples).  For example; [Application Insights React Demo](https://github.com/Azure-Samples/application-insights-react-demo)

### 1.3 Architecture Diagram
![Solution Architecture](https://github.com/rosscouldrey/Application-Insights-to-PowerBI-with-ADX/blob/2633b6ac066e5f6f39fc53035cf62c853041dbfa/Images/AppInsights%20to%20PowerBI%20using%20ADX%20Architecture.png)

## 2.0 Deployment and Usage
### 2.1 Configure application for logging

This repo will not cover how to deploy the app or configure it for Application Insights.  For a sample application please use one of these sample apps provided by Azure-Samples; <br>

  - [Application Insights React Demo](https://github.com/Azure-Samples/application-insights-react-demo) <br>
  - [Application Insights Click Plugin Demo](https://github.com/Azure-Samples/Application-Insights-Click-Plugin-Demo)

### 2.2 Deploy Azure Resources

*instructions for creating resources available [here](https://github.com/rosscouldrey/Application-Insights-to-PowerBI-with-ADX/blob/main/README.md#30-resources)*
  
For this architecture you must deploy the following resources;

1) Log Analytics Workspace (LAW) 
2) Application Insights (Workspace-based)
    - connect your App Insights to your LAW created above
4) Event Hubs
5) Azure Data Explorer (ADX)

### 2.3 Set up Azure Data Explorer

#### 2.31 Create Database

Create a new database inside your ADX cluster <br>
[Instructions to Create an ADX Database](https://docs.microsoft.com/en-us/azure/data-explorer/create-cluster-database-portal#create-a-database)

#### 2.32 Create tables and ingestion mapping

For this demo we use a landing table which captures all data pushed into ADX from the Event Hub.

The KQL queries to create the landing table and ingestion mapping are provided [here](https://github.com/rosscouldrey/Application-Insights-to-PowerBI-with-ADX/blob/main/KQL%20Scripts/1.%20LandingTable_setup.csl)

#### 2.33 Create the events tables, parsing functions and policies

We then use functions to parse the landed data which are invoked as data lands via policies.
For this demo we've decided to parse only 2 event types; PageViews and Events, which we land in their respective tables

However, you could chose to parse as many events as you'd like (the parsing function needs to be written beyond what is provided in this repo), or even parse them to a single events table if you'd prefer.

The KQL code for creating the events tables and the parsing functions are provided:
- [PageViewEvents](https://github.com/rosscouldrey/Application-Insights-to-PowerBI-with-ADX/blob/main/KQL%20Scripts/2.%20PageViewTable_setup.csl)
- [AppEvents](https://github.com/rosscouldrey/Application-Insights-to-PowerBI-with-ADX/blob/main/KQL%20Scripts/3.%20AppEventTable_setup.csl)

### 2.4 Connect the Event Hub to ADX via data connection

With our tables and ingestion mapping created, we can create a data connection between our Event Hub and our ADX database.

To do this, go to your database, select "Data connections" from the blade on the left and " + Add data connection". <br>
[Full Instructions here](https://docs.microsoft.com/en-us/azure/data-explorer/ingest-data-event-hub#connect-to-the-event-hub)

In the Target table section (if you've used the values from the scripts);

Table name = landing <br>
Data format = json <br>
Mapping name = landingMapping <br>

### 2.5 Start your sample app and create events

At this point we can begin to create events to log.  If you're using the sample apps provided, follow the directions to start the app, then visit the app and click around to generate events.

### 2.6 Test the data ingestion via KQL queries

You can use simple KQL to check that events are landing in your landing table, and subsequently being parsed into your events tables;

*test for data in landing table*
```  Kusto
  landing
  | limit(5)
```

_test for data in pageviews table_
``` Kusto
PageViewEvents
| limit(5)
```

You should see results from both of these queries.

### 2.6 Set up PowerBI Direct Query with Custom M Parameters

A [sample PBIT file](https://github.com/rosscouldrey/Application-Insights-to-PowerBI-with-ADX/blob/main/PBI%20Dynamic%20Query.pbit) is available in this repository to demonstrate how to connect PowerBI to ADX utilizing Dynamic M Query Parameters.  For this demo we've elected to use a timespan parameter to control how old the events to be ingested in our report should be.

The query that is used to pull the events is based on this simple query:
``` Kusto
PageViewEvents
| project Name, ResourceID, Timestamp_UTC, operationName, SessionID, refURI, DurationMs,PerformanceBucket
| where Timestamp_UTC > ago(1d)
```
With a parameter replacing the "1d" in the PowerBI file.

For a detailed write up on how to use Dynamic M Parameters refer to the [Resources](https://github.com/rosscouldrey/Application-Insights-to-PowerBI-with-ADX#30-resources) section of this repo.

The parameters for the PBIT file can be found in your created ADX instance;


_ADX Cluster_  <br>
Copy the URI from the overview page of your ADX resource.  This should be fully qualified <br> (Eg. https://**MYADXCLUSTER**.canadacentral.kusto.windows.net)

_ADX Database_ <br>
This is the name of the ADX database you created.  You can find the name on the Databases blade of your ADX resoruce in the Azure Portal.

_EventTable_<br>
This is the name of the event table to ingest; eg. **PageViewEvents**

_TimeToView_ <br>
This is a [timespan](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/scalar-data-types/timespan) parameter and should be entered as such.
eg. 1d

## 3.0 Resources

#### 3.1 Log Analytics Workspace
[Create a Log Analytics workspace](https://docs.microsoft.com/en-us/azure/azure-monitor/logs/quick-create-workspace?tabs=azure-portal)

#### 3.2 Application Insights
[Create an Application Insights resource](https://docs.microsoft.com/en-us/azure/azure-monitor/app/create-workspace-resource)

#### 3.3 Event Hubs
[Create an event hub using Azure portal](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create)

#### 3.4 Azure Data Explorer (ADX)
[Create an Azure Data Explorer cluster and database](https://docs.microsoft.com/en-us/azure/data-explorer/create-cluster-database-portal)

#### 3.4 Dynamic M Query Parameters in PowerBI
[Dynamic M query parameters in Power BI Desktop](https://docs.microsoft.com/en-us/power-bi/connect-data/desktop-dynamic-m-query-parameters)

#### 3.5 KQL References
[KQL Quick Reference](https://docs.microsoft.com/en-us/azure/data-explorer/kql-quick-reference)
