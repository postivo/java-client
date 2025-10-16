# Metadata
(*metadata()*)

## Overview

### Available Operations

* [list](#list) - List metadata
* [getPredefinedConfigs](#getpredefinedconfigs) - List predefined configs

## list

Retrieve a complete list of all types for shipments and their possible values. The method has no body and takes no parameters. The response includes metadata such as carrier types, service types, and more.

### Example Usage

<!-- UsageSnippet language="java" operationID="listMetadata" method="get" path="/metadata" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.ListMetadataResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        ListMetadataResponse res = sdk.metadata().list()
                .call();

        if (res.metadataResponse().isPresent()) {
            // handle response
        }
    }
}
```

### Response

**[ListMetadataResponse](../../models/operations/ListMetadataResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 400, 401, 403, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## getPredefinedConfigs

Retrieve a complete list of predefined shipment configurations. The method has no body and takes no parameters. The response includes all stored configuration options.

### Example Usage

<!-- UsageSnippet language="java" operationID="listPredefinedConfigs" method="get" path="/metadata/predefined-configs" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.ListPredefinedConfigsResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        ListPredefinedConfigsResponse res = sdk.metadata().getPredefinedConfigs()
                .call();

        if (res.predefinedConfigs().isPresent()) {
            // handle response
        }
    }
}
```

### Response

**[ListPredefinedConfigsResponse](../../models/operations/ListPredefinedConfigsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 400, 401, 403, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |