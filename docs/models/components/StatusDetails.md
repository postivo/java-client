# StatusDetails

Details of a single shipment and its status events


## Fields

| Field                                                                    | Type                                                                     | Required                                                                 | Description                                                              |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------ | ------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| `shipmentDetails`                                                        | [Optional\<ShipmentDetails>](../../models/components/ShipmentDetails.md) | :heavy_minus_sign:                                                       | Single shipment details                                                  |
| `statusEvents`                                                           | List\<[StatusEvent](../../models/components/StatusEvent.md)>             | :heavy_minus_sign:                                                       | List of status events for the shipment.                                  |