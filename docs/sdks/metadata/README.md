# Metadata

## Overview

### Available Operations

* [list](#list) - List metadata
* [getPredefinedConfigs](#getpredefinedconfigs) - List predefined configs

## list

Retrieve available shipment metadata, including carriers, services, paper types, envelopes and other configuration options.

### Example Usage

<!-- UsageSnippet language="java" operationID="listMetadata" method="get" path="/metadata" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.ListMetadataResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        ListMetadataResponse res = sdk.metadata().list()
                .call();

        if (res.metadataResponse().isPresent()) {
            System.out.println(res.metadataResponse().get());
        }
    }
}
```

### Response

**[ListMetadataResponse](../../models/operations/ListMetadataResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 4XX               | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## getPredefinedConfigs

Retrieve the predefined shipment configuration profiles stored in your account, including their configuration options.

### Example Usage

<!-- UsageSnippet language="java" operationID="listPredefinedConfigs" method="get" path="/metadata/predefined-configs" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.ListPredefinedConfigsResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        ListPredefinedConfigsResponse res = sdk.metadata().getPredefinedConfigs()
                .call();

        if (res.predefinedConfigs().isPresent()) {
            System.out.println(res.predefinedConfigs().get());
        }
    }
}
```

### Response

**[ListPredefinedConfigsResponse](../../models/operations/ListPredefinedConfigsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 4XX               | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |