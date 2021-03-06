<properties
    pageTitle="Create a Service Bus namespace with Event Hub and consumer group | Microsoft Azure"
    description="Create a Service Bus namespace with Event Hub and consumer group using ARM template"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="04/15/2016"
    ms.author="sethm;shvija"/>

# Create a Service Bus namespace with Event Hub and consumer group using an ARM template

This article shows how to use an Azure Resource Manager (ARM) template that creates a Service Bus namespace with an Event Hub and a consumer group. You will learn how to define which resources are deployed and how to define parameters that are specified when the deployment is executed. You can use this template for your own deployments, or customize it to meet your requirements

For more information about creating templates, please see [Authoring Azure Resource Manager Templates][].

For the complete template, see the [Service Bus Event Hub and consumer group template][] on GitHub.

>[AZURE.NOTE] The following ARM templates are available for download and deployment.
>
>-    [Create a Service Bus namespace with queue and authorization rule](service-bus-arm-namespace-auth-rule.md)
>-    [Create a Service Bus namespace with queue](service-bus-arm-namespace-queue.md)
>-    [Create a Service Bus namespace with topic and subscription](service-bus-arm-namespace-topic.md)
>-    [Create a Service Bus namespace](service-bus-arm-namespace.md)
>
>To check for the latest templates, see the [Azure Quickstart Templates][] and search for Service Bus.

## What will you deploy?

With this template, you will deploy a Service Bus namespace with an Event Hub and a consumer group.

Event Hubs is an event processing service used to provide event and telemetry ingress to Azure at massive scale, with low latency and high reliability.

Learn more about [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).

To run the deployment automatically, click the following button:

[![Deploy to Azure](./media/service-bus-arm-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-eventhub-and-consumergroup%2Fazuredeploy.json)

## Parameters

With Azure Resource Manager, you define parameters for values you want to specify when the template is deployed. The template includes a section called `Parameters` that contains all of the parameter values. You should define a parameter for those values that will vary based on the project you are deploying or based on the environment you are deploying to. Do not define parameters for values that will always stay the same. Each parameter value is used in the template to define the resources that are deployed.

We will describe each parameter in the template.

### serviceBusNamespaceName

The name of the Service Bus namespace to create.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### serviceBusEventHubName

The name of the Event Hub created in the Service Bus namespace.

```
" serviceBusEventHubName": {
"type": "string"
}
```

### serviceBusConsumerGroupName

The name of the consumer group created for the Event Hub in the Service Bus namespace.

```
" serviceBusConsumerGroupName": {
"type": "string"
}
```

### serviceBusApiVersion

The Service Bus API version of the template.

```
"serviceBusApiVersion": {
"type": "string"
}
```

## Resources to deploy

Creates a standard Service Bus namespace with an Event Hub and a consumer group.

```
"resources ": [
                {
                    "apiVersion": "[variables('ehVersion')]",
                    "name": "[parameters('serviceBusEventHubName')]",
                    "type": "EventHubs",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusEventHubName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('ehVersion')]",
                            "name": "[parameters('serviceBusConsumerGroupName')]",
                            "type": "ConsumerGroups",
                            "dependsOn": [
                                "[parameters('serviceBusEventHubName')]"
                            ],
                            "properties": {
                            }
                        }
                    ]
                }
            ]
        }
    ]
```

## Commands to run deployment

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-eventhub-and-consumergroup/azuredeploy.json
```

## Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-eventhub-and-consumergroup/azuredeploy.json][]
```

## Next steps

Now that you've created and deployed resources using ARM, learn how to manage these resources by viewing these articles:

- [Manage Azure Service Bus using Azure Automation](service-bus-automation-manage.md)
- [Manage Event Hubs with PowerShell](service-bus-powershell-how-to-provision.md)
- [Manage Event Hubs resources with the Service Bus Explorer](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)

  [Authoring Azure Resource Manager Templates]: ../resource-group-authoring-templates.md
  [Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Service Bus Event Hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-eventhub-and-consumerGroup/
