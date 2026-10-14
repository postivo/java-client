# PingResponse

Postivo service availability and current API version.


## Fields

| Field                                                                          | Type                                                                           | Required                                                                       | Description                                                                    | Example                                                                        |
| ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
| `status`                                                                       | *Optional\<String>*                                                            | :heavy_minus_sign:                                                             | API service status. A successful ping returns OK.                              | OK                                                                             |
| `version`                                                                      | *Optional\<String>*                                                            | :heavy_minus_sign:                                                             | Current API version.                                                           | 1.1.1                                                                          |
| `sandbox`                                                                      | *Optional\<Boolean>*                                                           | :heavy_minus_sign:                                                             | Indicates whether the connection was established to the test system (SANDBOX). | false                                                                          |