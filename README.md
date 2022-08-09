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
    - connect your App Insights to your LAW created in step 1
4) Event Hubs
5) Azure Data Explorer (ADX)

### 2.3 Set up Azure Data Explorer

### 2.4 Set up PowerBI Direct Query with Custom M Parameters

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
