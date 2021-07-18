# home-assistant-azure-powerbi
Push from Home Assistant to PowerBI via Azure Event Hubs for Visualisation





# Azure Setup
## Setup the Azure Resources
Following commands run from bash and the azure cli.  If you don't have any, you can run them remotely from the Azure Cloud Shell: https://portal.azure.com/#cloudshell/

Commands are a variation from those provided from Azure tutorial (here)[https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-tutorial-visualize-anomalies]
``` bash
# Set the values for location and resource group name.
location=australiacentral
resourceGroup=CuttageeResourcesEH

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The Event Hubs namespace name must be globally unique, so add a random number to the end.
eventHubNamespace=CuttageeEHNamespace$RANDOM
echo "Event Hub Namespace = " $eventHubNamespace

# Create the Event Hubs namespace.
az eventhubs namespace create --resource-group $resourceGroup \
   --name $eventHubNamespace \
   --location $location \
   --sku Standard

# The event hub name must be globally unique, so add a random number to the end.
eventHubName=CuttageeEHhub$RANDOM
echo "event hub name = " $eventHubName

# Create the event hub.
az eventhubs eventhub create --resource-group $resourceGroup \
    --namespace-name $eventHubNamespace \
    --name $eventHubName \
    --message-retention 3 \
    --partition-count 2

# Get the connection string that authenticates the app with the Event Hubs service.
connectionString=$(az eventhubs namespace authorization-rule keys list \
   --resource-group $resourceGroup \
   --namespace-name $eventHubNamespace \
   --name RootManageSharedAccessKey \
   --query primaryConnectionString \
   --output tsv)
echo "Connection string = " $connectionString 

```

Take note of the connection string and the eventHubName as you're going to need them when pushing from Home Assistant later.


## Receive Data from Home Assistant




## Transform

## Push to PowerBI

# Home Assistance Config
Configure Home Assistant to Push Data to Azure Event Hubs.

Update the configuration.yaml for Home Assistant with Azure EventHubs configuration.  The official HA documentation can help if you need to do something different, or want to filter different entities: https://www.home-assistant.io/integrations/azure_event_hub/

``` yaml
# Push Data to Azure Event Hubs
azure_event_hub:
  event_hub_connection_string: !secret AZURE_CONNECTION_STRING
  event_hub_instance_name: !secret EVENT_HUB_INSTANCE_NAME
  send_interval: 60
  max_delay: 5
  # Only push temperature entities to start with
  filter: 
    # Include here all the entities you want to display, in my case it is the key temperature, aircon and power entities
    include_entity_globs:
      - sensor.*temp*
    include_entities: 
      - climate.aircon
      - sensor.solaredge_m1_ac_power
      - sensor.solaredge_ac_power

```

Adding the appropriate clauses to the secrets.yaml also: 
``` yaml
#Azure Private Info
AZURE_CONNECTION_STRING: Endpoint=sb://foo.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=bar
EVENT_HUB_INSTANCE_NAME: foobar
```

Restart Home Assistant to make change take effect.

# PowerBI Setup

TBC

# Housekeepting
Azure will cost money if you keep it online and don't use it, to minimise cost impact, consider:
## Turn off Everything

## Delete Everything

# References

Azure and PowerBI work: 
https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-tutorial-visualize-anomalies

Home Assistant work: 
https://www.home-assistant.io/integrations/azure_event_hub/
