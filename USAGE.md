<!-- Start SDK Example Usage [usage] -->
### Sending Shipment to single recipient

This example demonstrates simple sending Shipment to a single recipient:

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

### Checking the price of a shipment for single recipient

This example demonstrates simple checking the price of a Shipment to a single recipient:

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
<!-- End SDK Example Usage [usage] -->