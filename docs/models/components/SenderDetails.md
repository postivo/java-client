# SenderDetails

Extended sender details.


## Fields

| Field                                         | Type                                          | Required                                      | Description                                   | Example                                       |
| --------------------------------------------- | --------------------------------------------- | --------------------------------------------- | --------------------------------------------- | --------------------------------------------- |
| `id`                                          | *Optional\<Long>*                             | :heavy_minus_sign:                            | Unique sender ID.                             | 665                                           |
| `sender`                                      | [Sender](../../models/components/Sender.md)   | :heavy_check_mark:                            | Sender data for the shipment.                 |                                               |
| `active`                                      | *boolean*                                     | :heavy_check_mark:                            | Indicates whether the sender is active.       | true                                          |
| `default_`                                    | *boolean*                                     | :heavy_check_mark:                            | Indicates whether this is the default sender. | true                                          |