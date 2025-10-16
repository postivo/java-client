# RecipientFromAddressBook

Select recipient data for a shipment from the Address Book.


## Fields

| Field                                                                 | Type                                                                  | Required                                                              | Description                                                           | Example                                                               |
| --------------------------------------------------------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------- |
| `id`                                                                  | *long*                                                                | :heavy_check_mark:                                                    | Global ID of the recipient stored in the personal Address Book.       | 344                                                                   |
| `customId`                                                            | *JsonNullable\<String>*                                               | :heavy_minus_sign:                                                    | User-assigned custom shipment ID.                                     | my-id-1113                                                            |
| `postscript`                                                          | *JsonNullable\<String>*                                               | :heavy_minus_sign:                                                    | Optional postscript printed above the recipient data on the envelope. | Do rąk własnych                                                       |