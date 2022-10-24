# Alerts for Azure Landing Zone

One of the most common questions we've faced in working with Customers is, "What should we monitor in Azure?" and "What thresholds should we configure our alerts for?"

There hasn't been a definitive list of what you should monitor when you deploy something to Azure but the documentation for each Azure resource does a pretty good job of providing some recommendations, some of those recommedations are short simple metric queries, some are slightly more complex log alerts and sometimes there's a lot to read through such as with [Storage Accounts](https://learn.microsoft.com/en-us/azure/storage/blobs/blob-storage-monitoring-scenarios). Microsoft has also create a number of 'insight solutions' which pull together all the things you shoudl carea about for some resources ([Storage Insights](https://learn.microsoft.com/en-us/azure/storage/common/storage-insights-overview), [VM Insights](https://learn.microsoft.com/en-us/azure/azure-monitor/vm/vminsights-overview), [Container Insights](https://learn.microsoft.com/en-us/azure/azure-monitor/containers/container-insights-overview)); but what about everything else???

We approached this task by first focusing on monitoring the most common Azure resources found in Azure Landing zones because their pretty standard.

Do you need to have Azure Landing zones deployed for this to work?

*No but you will need to be using Azure Management groups.*

Do you need to use the thresholds we've defined in the metric rule alert?

*It's provided as a starting point, we've based the initial threshold on what we've seen and what Microsoft's documentation recommends. You will need to adjust the thresholds at some point. You'll need to observe and if the alert is too chatty, adjust the threshold up; if it's not alerting when there's a problem, adjust the threshold down a bit. The key thing is you'll need to investigate, leverage the insights if they are available, or create a workbook or dashboard to help you out.*

Do we need to use these metrics or can we replace them with other ones?

*The metric rules we've created are based on recommendations from Microsoft documentation and field exprience. How you're using Azure resources may also be different so tailor the alerts to suit your needs. One of the other goals of this project is to help you have a way to do Azure Monitor alerts at scale, create new rules with your own thresholds. We'd love to hear about your new rules too so feel free to share back.*

## Dependencies

This project uses the bicep modules from the [CARML](https://github.com/Azure/ResourceModules), version [0.7.0](https://github.com/Azure/ResourceModules/releases/tag/v0.7.0). We will work to keep this as compatible with the [CARML](https://github.com/Azure/ResourceModules) repo but for the moment our priority is to build this project up as much as possible so things may break if you use a more current version of the Bicep modules found in [CARML](https://github.com/Azure/ResourceModules).

## Roadmap

Our approach was to first tackle creating Metric alerts because they are responsive and alerts are relatively inexpensive because it's pre-computed and stored in the system, where as log alerts are stored in a Log Analytics Workspace and have had some sort of logic operation performed on the data. Click [here](https://learn.microsoft.com/en-us/azure/azure-monitor/alerts/alerts-types#metric-alerts) for more information on Metric alerts.

Next we're going to tackle Service Health alerts, knowing when there's an outage, planned maintenance and other health advisories for the services you're using. These types of alerts rely on information in the ActivityLog. Click [here](https://learn.microsoft.com/en-us/azure/service-health/overview) for more information on Service Health Alerts.

The final area we're going to tackle is log alerts and as stated earlier the data is collected in a Log Analytics Workspace and some sort of logic operation is performed on the data which means there's charge for using these types of alerts.

- Where possible we're going to focus on queries which use the [ScheduledQueryRules API](https://learn.microsoft.com/en-us/azure/azure-monitor/alerts/alerts-log-api-switch) and [Metric alerts for Logs](https://learn.microsoft.com/en-us/azure/azure-monitor/alerts/alerts-metric-logs).

- ActivityLog alerts we may create additional alerts based on data found in the ActivityLog Alerts that are not Service Health related.

## Prerequisites

VSCode
Bicep Extension
Azure subscriptions where you want to apply alerts.
Management Groups that manage the Azure subscriptions
...

## Deployment Steps

The intention of the policies is to provide a common set of metrics and thresholds to monitor all Azure Landing Zone resources which is why you will want to deploy this as high up in your Azure Management group structure to ensure that all subscriptions are included and the steps below will cover that specific scenario. If there are other scenarios you'd like to see or have applied the policies at a different level within your Azure Management group structure send us feedback through GitHub Issues. <<Link>>

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit <https://cla.opensource.microsoft.com>.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft
trademarks or logos is subject to and must follow
[Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.
