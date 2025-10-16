# Common
(*common()*)

## Overview

Common

### Available Operations

* [ping](#ping) - Check API availability and version

## ping

Check the API connection and current availability status. The response also includes the current API version.

### Example Usage

<!-- UsageSnippet language="java" operationID="ping" method="get" path="/ping" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.PingResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
            .build();

        PingResponse res = sdk.common().ping()
                .call();

        if (res.pingResponse().isPresent()) {
            // handle response
        }
    }
}
```

### Response

**[PingResponse](../../models/operations/PingResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 400, 4XX                    | application/problem+json    |
| models/errors/ErrorResponse | 503, 5XX                    | application/problem+json    |