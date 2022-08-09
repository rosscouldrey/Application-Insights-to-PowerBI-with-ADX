# Application-Insights-to-PowerBI-with-ADX

## Solution Summary
### Overview
This solution was architected to provide application telemetry reporting through PowerBI utilizing Azure Application Insights and Azure Data Explorer.  Application logs are pushed to Azure Data Explorer through an Event Hub utilizing diagnostic settings.  Then PowerBI is connected to ADX utilizing Custom M Query Parameters and direct query.

This solution should be considered in cases where log files are too large to ingest into PowerBI via "import" mode.

### Prerequisites
To implement this solution you will need access to an Azure environment with the ability to create resources.  Set up a free azure account [here](https://azure.microsoft.com/en-ca/free/search/?OCID=AIDcmmqz3gd78m_SEM_af052705cd091cba049645f7a218ed35:G:s&ef_id=af052705cd091cba049645f7a218ed35:G:s&msclkid=af052705cd091cba049645f7a218ed35)


This solution requires an application to generate events which are sent to Application Insights.  For sample applications that can generate events please visit the [Azure-Samples github](https://github.com/Azure-Samples).  For example; [Application Insights React Demo](https://github.com/Azure-Samples/application-insights-react-demo)

### Architecture Diagram
![Solution Architecture](https://github.com/rosscouldrey/Application-Insights-to-PowerBI-with-ADX/blob/2633b6ac066e5f6f39fc53035cf62c853041dbfa/Images/AppInsights%20to%20PowerBI%20using%20ADX%20Architecture.png)

## Deployment and Usage
### Configure application for logging

This repo will not cover how to deploy the app or configure it for Application Insights.  For a sample application please use one of these sample apps provided by Azure-Samples; 
[Application Insights React Demo](https://github.com/Azure-Samples/application-insights-react-demo)
[Application Insights Click Plugin Demo](https://github.com/Azure-Samples/Application-Insights-Click-Plugin-Demo)

### Deploy Azure Resources

For this architecture you must deploy the following resources;
1) Log Analytics Workspace (LAW)
2) Application Insights (Workspace-based)
      a) connect your App Insights to your LAW created in step 1
3) Event Hubs
4) Azure Data Explorer (ADX)

### Set up Azure Data Explorer

### Set up PowerBI Direct Query with Custom M Parameters

## Resources

#### Application Insights
[Create an Application Insights resource](https://docs.microsoft.com/en-us/azure/azure-monitor/app/create-workspace-resource)

#### Event Hubs
[Create an event hub using Azure portal](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create)

#### Azure Data Explorer (ADX)
[Create an Azure Data Explorer cluster and database](https://docs.microsoft.com/en-us/azure/data-explorer/create-cluster-database-portal)

#### Dynamic M Query Parameters in PowerBI
[Dynamic M query parameters in Power BI Desktop](https://docs.microsoft.com/en-us/power-bi/connect-data/desktop-dynamic-m-query-parameters)
