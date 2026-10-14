# Shipments

## Overview

### Available Operations

* [status](#status) - Retrieve shipment details with status events
* [cancel](#cancel) - Cancel shipments
* [dispatch](#dispatch) - Dispatch a new shipment
* [documents](#documents) - Retrieve documents related to a shipment
* [price](#price) - Check the shipment price

## status

Retrieve the current status and details for up to **50 shipments** using the IDs assigned when they were created. Separate multiple IDs with commas. Duplicate IDs are processed once.

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

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        GetStatusResponse res = sdk.shipments().status()
                .ids(List.of(
                    "A0043456"))
                .call();

        if (res.statusDetails().isPresent()) {
            System.out.println(res.statusDetails().get());
        }
    }
}
```

### Parameters

| Parameter                                                                                                                                                                                                                                                                            | Type                                                                                                                                                                                                                                                                                 | Required                                                                                                                                                                                                                                                                             | Description                                                                                                                                                                                                                                                                          |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `ids`                                                                                                                                                                                                                                                                                | List\<*String*>                                                                                                                                                                                                                                                                      | :heavy_check_mark:                                                                                                                                                                                                                                                                   | Shipment IDs assigned by the system (comma-separated). The system accepts a maximum of **50** identifiers per call. Empty items, including leading, trailing or repeated commas, are rejected with HTTP 400. Surrounding whitespace is ignored and duplicate IDs are processed once. |

### Response

**[GetStatusResponse](../../models/operations/GetStatusResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 404, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## cancel

Cancel up to **50 shipments** using the IDs assigned when they were created. Separate multiple IDs with commas. Only shipments with status `ACCEPTED` can be cancelled; duplicate IDs are processed once.

Check the result for each shipment, as individual cancellations can succeed or fail independently. If processing is interrupted, earlier cancellations may already have completed; check shipment status before retrying.

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

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        CancelShipmentResponse res = sdk.shipments().cancel()
                .ids(List.of(
                    "A0043456"))
                .call();

        if (res.shipmentCancellations().isPresent()) {
            System.out.println(res.shipmentCancellations().get());
        }
    }
}
```

### Parameters

| Parameter                                                                                                                                                                                                                                                                            | Type                                                                                                                                                                                                                                                                                 | Required                                                                                                                                                                                                                                                                             | Description                                                                                                                                                                                                                                                                          |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `ids`                                                                                                                                                                                                                                                                                | List\<*String*>                                                                                                                                                                                                                                                                      | :heavy_check_mark:                                                                                                                                                                                                                                                                   | Shipment IDs assigned by the system (comma-separated). The system accepts a maximum of **50** identifiers per call. Empty items, including leading, trailing or repeated commas, are rejected with HTTP 400. Surrounding whitespace is ignored and duplicate IDs are processed once. |

### Response

**[CancelShipmentResponse](../../models/operations/CancelShipmentResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 404, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## dispatch

Submit shipments for up to **50 recipients** using their addresses or Address Book identifiers, documents and optional shipment settings. For larger batches, split the operation into multiple calls. Check each recipient result, as individual shipments can succeed or fail independently.

### Example Usage: advanced_config

<!-- UsageSnippet language="java" operationID="shipmentDispatch" method="post" path="/shipment" example="advanced_config" -->
```java
package hello.world;

import java.lang.Exception;
import java.time.LocalDate;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.components.*;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.ShipmentDispatchResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

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
                .documents(ShipmentDocuments.of(Documents.of(DocumentPdf.builder()
                    .fileStream("<document content encoded to base64>")
                    .fileName("document.pdf")
                    .build())))
                .options(Options.of(ShipmentOptions.builder()
                    .predefinedConfigId(2670L)
                    .inlineConfig(InlineConfigUnion.of(InlineConfig.builder()
                        .carrierId(4L)
                        .serviceId(16L)
                        .paperId(1L)
                        .colorPrint(true)
                        .duplexPrint(false)
                        .envelopeId(454L)
                        .envelopeColorPrint(true)
                        .build()))
                    .senderId(1L)
                    .dispatchDate(LocalDate.parse("2025-10-21"))
                    .callback(CallbackUnion.of(Callback.builder()
                        .url("https://example.com/test")
                        .userToken("75gh28hugjy8gfv6...")
                        .build()))
                    .miscInfo(MiscInfoUnion.of(MiscInfo.builder()
                        .mpk("dział sprzedaży")
                        .orderName("Wysyłka zaproszeń do Klientów")
                        .build()))
                    .rotateDocuments(true)
                    .build()))
                .build();

        ShipmentDispatchResponse res = sdk.shipments().dispatch()
                .request(req)
                .call();

        if (res.shipmentDetails().isPresent()) {
            System.out.println(res.shipmentDetails().get());
        }
    }
}
```
### Example Usage: multi

<!-- UsageSnippet language="java" operationID="shipmentDispatch" method="post" path="/shipment" example="multi" -->
```java
package hello.world;

import java.lang.Exception;
import java.util.List;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.components.*;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.ShipmentDispatchResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        Shipment req = Shipment.builder()
                .recipients(ShipmentRecipients.of(List.of(
                    Recipients.of(RecipientInline.builder()
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
                        .build()),
                    Recipients.of(RecipientInline.builder()
                        .name("Andrzej Kowalski")
                        .address("ul. Przykładowa 45/321")
                        .postCode("34-001")
                        .city("Kraków")
                        .phoneNumber("+48999888777")
                        .customId("my-id-1113")
                        .build()))))
                .documents(ShipmentDocuments.of(Documents.of(DocumentPdf.builder()
                    .fileStream("<document content encoded to base64>")
                    .fileName("document.pdf")
                    .build())))
                .options(Options.of(ShipmentOptions.builder()
                    .predefinedConfigId(2670L)
                    .build()))
                .build();

        ShipmentDispatchResponse res = sdk.shipments().dispatch()
                .request(req)
                .call();

        if (res.shipmentDetails().isPresent()) {
            System.out.println(res.shipmentDetails().get());
        }
    }
}
```
### Example Usage: single

<!-- UsageSnippet language="java" operationID="shipmentDispatch" method="post" path="/shipment" example="single" -->
```java
package hello.world;

import java.lang.Exception;
import java.util.List;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.components.*;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.ShipmentDispatchResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

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
            System.out.println(res.shipmentDetails().get());
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
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 415, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## documents

Retrieve a shipment document using the shipment ID and document type. Available documents include dispatch confirmations, envelope previews, EPO certificate visualizations in PDF format and EPO certificates in XML format. Availability depends on the document type and shipment state.

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

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        GetDocumentsResponse res = sdk.shipments().documents()
                .id("A0043456")
                .type(DocumentType.DISPATCH_CERT)
                .call();

        if (res.documentResponse().isPresent()) {
            System.out.println(res.documentResponse().get());
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
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 404, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## price

Calculate shipment prices for up to **50 recipients** using their addresses or Address Book identifiers, documents and optional shipment settings. For larger batches, split the operation into multiple calls. Check each recipient result to determine whether pricing succeeded.

### Example Usage: advanced_config

<!-- UsageSnippet language="java" operationID="shipmentPrice" method="post" path="/shipment/price" example="advanced_config" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.components.*;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.ShipmentPriceResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

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
                .documents(ShipmentDocuments.of(Documents.of(DocumentPdf.builder()
                    .fileStream("<document content encoded to base64>")
                    .fileName("document.pdf")
                    .build())))
                .options(Options.of(ShipmentOptions.builder()
                    .predefinedConfigId(2670L)
                    .inlineConfig(InlineConfigUnion.of(InlineConfig.builder()
                        .carrierId(4L)
                        .serviceId(16L)
                        .paperId(1L)
                        .colorPrint(true)
                        .duplexPrint(false)
                        .envelopeId(454L)
                        .envelopeColorPrint(true)
                        .build()))
                    .senderId(1L)
                    .build()))
                .build();

        ShipmentPriceResponse res = sdk.shipments().price()
                .request(req)
                .call();

        if (res.shipmentPrices().isPresent()) {
            System.out.println(res.shipmentPrices().get());
        }
    }
}
```
### Example Usage: multi

<!-- UsageSnippet language="java" operationID="shipmentPrice" method="post" path="/shipment/price" example="multi" -->
```java
package hello.world;

import java.lang.Exception;
import java.util.List;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.components.*;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.ShipmentPriceResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        Shipment req = Shipment.builder()
                .recipients(ShipmentRecipients.of(List.of(
                    Recipients.of(RecipientInline.builder()
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
                        .build()),
                    Recipients.of(RecipientInline.builder()
                        .name("Andrzej Kowalski")
                        .address("ul. Przykładowa 45/321")
                        .postCode("34-001")
                        .city("Kraków")
                        .phoneNumber("+48999888777")
                        .customId("my-id-1113")
                        .build()))))
                .documents(ShipmentDocuments.of(Documents.of(DocumentPdf.builder()
                    .fileStream("<document content encoded to base64>")
                    .fileName("document.pdf")
                    .build())))
                .options(Options.of(ShipmentOptions.builder()
                    .predefinedConfigId(2670L)
                    .build()))
                .build();

        ShipmentPriceResponse res = sdk.shipments().price()
                .request(req)
                .call();

        if (res.shipmentPrices().isPresent()) {
            System.out.println(res.shipmentPrices().get());
        }
    }
}
```
### Example Usage: single

<!-- UsageSnippet language="java" operationID="shipmentPrice" method="post" path="/shipment/price" example="single" -->
```java
package hello.world;

import java.lang.Exception;
import java.util.List;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.components.*;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.ShipmentPriceResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

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
            System.out.println(res.shipmentPrices().get());
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
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 415, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |