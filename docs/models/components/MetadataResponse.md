# MetadataResponse

Metadata response.


## Fields

| Field                                                                                | Type                                                                                 | Required                                                                             | Description                                                                          |
| ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------ |
| `carriers`                                                                           | List\<[MetadataResponseCarrier](../../models/components/MetadataResponseCarrier.md)> | :heavy_minus_sign:                                                                   | List of carriers and their available services.                                       |
| `papers`                                                                             | List\<[Paper](../../models/components/Paper.md)>                                     | :heavy_minus_sign:                                                                   | Available paper types.                                                               |
| `envelopeTemplates`                                                                  | List\<[EnvelopeTemplate](../../models/components/EnvelopeTemplate.md)>               | :heavy_minus_sign:                                                                   | Envelope template groups, each containing related templates.                         |
| `statusCodes`                                                                        | List\<[StatusCode](../../models/components/StatusCode.md)>                           | :heavy_minus_sign:                                                                   | Available status codes.                                                              |