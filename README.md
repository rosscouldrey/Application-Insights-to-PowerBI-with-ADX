# Application-Insights-to-PowerBI-with-ADX

## Solution Summary
### Overview
This solution was architected to provide application telemetry reporting through PowerBI utilizing Azure Application Insights and Azure Data Explorer

### Prerequisites
To implement this solution you will need access to an Azure environment with the ability to create resources.  Set up a free azure account [here](https://azure.microsoft.com/en-ca/free/search/?OCID=AIDcmmqz3gd78m_SEM_af052705cd091cba049645f7a218ed35:G:s&ef_id=af052705cd091cba049645f7a218ed35:G:s&msclkid=af052705cd091cba049645f7a218ed35)


This solution requires an application to generate events which are sent to Application Insights.  For sample applications that can generate events please visit the [Azure-Samples github](https://github.com/Azure-Samples).  For example; [Application Insights React Demo](https://github.com/Azure-Samples/application-insights-react-demo)

### Architecture Diagram
![Solution Architecture](https://github.com/rosscouldrey/Application-Insights-to-PowerBI-with-ADX/blob/2633b6ac066e5f6f39fc53035cf62c853041dbfa/Images/AppInsights%20to%20PowerBI%20using%20ADX%20Architecture.png)

## Resources

#### Application Insights
[Create an Application Insights resource](https://docs.microsoft.com/en-us/azure/azure-monitor/app/create-workspace-resource)

#### Event Hubs
[Create an event hub using Azure portal](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create)

#### Azure Data Explorer (ADX)
[Create an Azure Data Explorer cluster and database](https://docs.microsoft.com/en-us/azure/data-explorer/create-cluster-database-portal)
