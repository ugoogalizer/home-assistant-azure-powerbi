# home-assistant-azure-powerbi
Push from Home Assistant to PowerBI via Azure Event Hubs for Visualisation





# Azure Setup
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
    include_entity_globs:
      - sensor.*temp*
```

Adding the appropriate clauses to the secrets.yaml also: 
``` yaml
#Azure Private Info
AZURE_CONNECTION_STRING: Endpoint=sb://foo.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=bar
EVENT_HUB_INSTANCE_NAME: foobar
```


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
