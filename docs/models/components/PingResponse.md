# PingResponse

Ping response.


## Fields

| Field                                                                          | Type                                                                           | Required                                                                       | Description                                                                    | Example                                                                        |
| ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
| `status`                                                                       | *Optional\<String>*                                                            | :heavy_minus_sign:                                                             | API service status: OK (available), ERR (unavailable).                         | OK                                                                             |
| `version`                                                                      | *Optional\<String>*                                                            | :heavy_minus_sign:                                                             | Current API version.                                                           | 1.0                                                                            |
| `sandbox`                                                                      | *Optional\<Boolean>*                                                           | :heavy_minus_sign:                                                             | Indicates whether the connection was established to the test system (SANDBOX). | false                                                                          |