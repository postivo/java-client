# Shipments
(*shipments()*)

## Overview

### Available Operations

* [status](#status) - Retrieve shipment details with status events
* [cancel](#cancel) - Cancel shipments
* [dispatch](#dispatch) - Dispatch a new shipment
* [documents](#documents) - Retrieve documents related to a shipment
* [price](#price) - Check the shipment price

## status

Retrieve the current status and details for one or more shipments by their `ids`. Pass the unique shipment IDs (returned when the shipments were created) as a path parameter. To query provide a list (up to **50** IDs per call). For larger batches, split the requests.

### Example Usage

<!-- UsageSnippet language="java" operationID="getStatus" method="get" path="/shipment/{ids}" -->
```java
package hello.world;

import java.lang.Exception;
import java.util.List;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.GetStatusResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        GetStatusResponse res = sdk.shipments().status()
                .ids(List.of(
                    "A0043456"))
                .call();

        if (res.statusDetails().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                                           | Type                                                                                                                | Required                                                                                                            | Description                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| `ids`                                                                                                               | List\<*String*>                                                                                                     | :heavy_check_mark:                                                                                                  | Shipment IDs assigned by the system (comma-separated). The system accepts a maximum of **50** identifiers per call. |

### Response

**[GetStatusResponse](../../models/operations/GetStatusResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 400, 401, 403, 404, 4XX     | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## cancel

Cancel shipments that have not yet been processed by their `ids`. Pass the unique shipment IDs (returned when the shipment was created) as a parameter. To cancel multiple shipments at once, provide a list of IDs (up to **50** per call). For larger volumes, split the operation into multiple requests. Only shipments with status `ACCEPTED` can be cancelled.

If duplicate shipment IDs are provided in a single call, the API processes each unique ID only once.

### Example Usage

<!-- UsageSnippet language="java" operationID="cancelShipment" method="delete" path="/shipment/{ids}" -->
```java
package hello.world;

import java.lang.Exception;
import java.util.List;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.CancelShipmentResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        CancelShipmentResponse res = sdk.shipments().cancel()
                .ids(List.of(
                    "A0043456"))
                .call();

        if (res.shipmentCancellations().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                                           | Type                                                                                                                | Required                                                                                                            | Description                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| `ids`                                                                                                               | List\<*String*>                                                                                                     | :heavy_check_mark:                                                                                                  | Shipment IDs assigned by the system (comma-separated). The system accepts a maximum of **50** identifiers per call. |

### Response

**[CancelShipmentResponse](../../models/operations/CancelShipmentResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 400, 401, 403, 404, 4XX     | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## dispatch

Send a shipment to one or multiple recipients in a single request. Provide a `Shipment` object. The object includes properties that define the shipment (recipient details, included documents, and optional settings). Some fields are required.

The system accepts up to **50** recipients per call. For larger volumes, split the operation into multiple requests.

### Example Usage

<!-- UsageSnippet language="java" operationID="shipmentDispatch" method="post" path="/shipment" -->
```java
package hello.world;

import java.lang.Exception;
import java.util.List;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.components.*;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.ShipmentDispatchResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        Shipment req = Shipment.builder()
                .recipients(ShipmentRecipients.of(Recipients.of(RecipientInline.builder()
                    .name("Jan Nowak")
                    .address("ul. Testowa")
                    .postCode("00-999")
                    .city("Warszawa")
                    .name2("Firma testowa Sp. z o.o.")
                    .homeNumber("23")
                    .flatNumber("2")
                    .phoneNumber("+48666666666")
                    .postscript("Komunikat")
                    .customId("1234567890")
                    .build())))
                .documents(ShipmentDocuments.of(List.of(
                    Documents.of(DocumentPdf.builder()
                        .fileStream("<document_1 content encoded to base64>")
                        .fileName("document1.pdf")
                        .build()),
                    Documents.of(DocumentPdf.builder()
                        .fileStream("<document_2 content encoded to base64>")
                        .fileName("document2.pdf")
                        .build()))))
                .options(Options.of(ShipmentOptions.builder()
                    .predefinedConfigId(2670L)
                    .build()))
                .build();

        ShipmentDispatchResponse res = sdk.shipments().dispatch()
                .request(req)
                .call();

        if (res.shipmentDetails().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                   | Type                                        | Required                                    | Description                                 |
| ------------------------------------------- | ------------------------------------------- | ------------------------------------------- | ------------------------------------------- |
| `request`                                   | [Shipment](../../models/shared/Shipment.md) | :heavy_check_mark:                          | The request object to use for the request.  |

### Response

**[ShipmentDispatchResponse](../../models/operations/ShipmentDispatchResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 400, 401, 403, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## documents

Download documents related to a shipment by its `id`. Pass the unique shipment `id` (returned when the shipment was created) as a parameter. The second parameter is the document type to download. Supported document types include: dispatch certificate, envelope template, and EPO (in PDF or XML formats).

### Example Usage

<!-- UsageSnippet language="java" operationID="getDocuments" method="get" path="/shipment/{id}/document/{type}" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.DocumentType;
import pl.postivo.sdk.models.operations.GetDocumentsResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        GetDocumentsResponse res = sdk.shipments().documents()
                .id("A0043456")
                .type(DocumentType.DISPATCH_CERT)
                .call();

        if (res.documentResponse().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                | Type                                                                     | Required                                                                 | Description                                                              | Example                                                                  |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------ | ------------------------------------------------------------------------ | ------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| `id`                                                                     | *String*                                                                 | :heavy_check_mark:                                                       | Single shipment ID assigned by the system when the shipment was created. | A0043456                                                                 |
| `type`                                                                   | [DocumentType](../../models/operations/DocumentType.md)                  | :heavy_check_mark:                                                       | Type of document/certificate to generate.                                | dispatch_cert                                                            |

### Response

**[GetDocumentsResponse](../../models/operations/GetDocumentsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 400, 401, 403, 404, 4XX     | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## price

Check the price of a shipment for one or multiple recipients. Provide a `Shipment` object in the request. Each object includes properties such as recipient details, included documents, and optional settings. Some fields are required.

The system accepts up to **50** recipients per call. For larger volumes, split the operation into multiple requests.

### Example Usage

<!-- UsageSnippet language="java" operationID="shipmentPrice" method="post" path="/shipment/price" -->
```java
package hello.world;

import java.lang.Exception;
import java.util.List;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.components.*;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.ShipmentPriceResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        Shipment req = Shipment.builder()
                .recipients(ShipmentRecipients.of(Recipients.of(RecipientInline.builder()
                    .name("Jan Nowak")
                    .address("ul. Testowa")
                    .postCode("00-999")
                    .city("Warszawa")
                    .name2("Firma testowa Sp. z o.o.")
                    .homeNumber("23")
                    .flatNumber("2")
                    .phoneNumber("+48666666666")
                    .postscript("Komunikat")
                    .customId("1234567890")
                    .build())))
                .documents(ShipmentDocuments.of(List.of(
                    Documents.of(DocumentPdf.builder()
                        .fileStream("<document_1 content encoded to base64>")
                        .fileName("document1.pdf")
                        .build()),
                    Documents.of(DocumentPdf.builder()
                        .fileStream("<document_2 content encoded to base64>")
                        .fileName("document2.pdf")
                        .build()))))
                .options(Options.of(ShipmentOptions.builder()
                    .predefinedConfigId(2670L)
                    .build()))
                .build();

        ShipmentPriceResponse res = sdk.shipments().price()
                .request(req)
                .call();

        if (res.shipmentPrices().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                   | Type                                        | Required                                    | Description                                 |
| ------------------------------------------- | ------------------------------------------- | ------------------------------------------- | ------------------------------------------- |
| `request`                                   | [Shipment](../../models/shared/Shipment.md) | :heavy_check_mark:                          | The request object to use for the request.  |

### Response

**[ShipmentPriceResponse](../../models/operations/ShipmentPriceResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 400, 401, 403, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |