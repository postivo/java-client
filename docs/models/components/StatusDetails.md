# StatusDetails

Details of a single shipment and its status events


## Fields

| Field                                                         | Type                                                          | Required                                                      | Description                                                   |
| ------------------------------------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------- |
| `shipmentDetails`                                             | [ShipmentDetails](../../models/components/ShipmentDetails.md) | :heavy_check_mark:                                            | Single shipment details                                       |
| `statusEvents`                                                | List\<[StatusEvent](../../models/components/StatusEvent.md)>  | :heavy_check_mark:                                            | List of status events for the shipment.                       |