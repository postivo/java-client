[![Maven Central Version](https://img.shields.io/maven-central/v/pl.postivo/client)](https://central.sonatype.com/artifact/pl.postivo/client)
[![GitHub License](https://img.shields.io/github/license/postivo/java-client)](https://github.com/postivo/java-client/blob/main/LICENSE)
[![Static Badge](https://img.shields.io/badge/built_by-Speakeasy-yellow)](https://www.speakeasy.com/?utm_source=openapi&utm_campaign=java)
# POSTIVO.PL REST API Client SDK for JAVA

This package provides the **POSTIVO.PL Hybrid Mail Services SDK** for JAVA, allowing you to dispatch shipments directly from your application via the [POSTIVO.PL](https://postivo.pl) platform.

## Additional documentation:

Comprehensive documentation of all methods and types is available below in [Available Resources and Operations](#available-resources-and-operations).

You can also refer to the [REST API v1 documentation](https://api.postivo.pl/rest/v1/) for additional details about this SDK.

<!-- No Summary [summary] -->

<!-- Start Table of Contents [toc] -->
## Table of Contents
<!-- $toc-max-depth=2 -->
* [POSTIVO.PL REST API Client SDK for JAVA](#postivopl-rest-api-client-sdk-for-java)
  * [Additional documentation:](#additional-documentation)
  * [SDK Installation](#sdk-installation)
  * [SDK Example Usage](#sdk-example-usage)
  * [Asynchronous Support](#asynchronous-support)
  * [Authentication](#authentication)
  * [Available Resources and Operations](#available-resources-and-operations)
  * [Retries](#retries)
  * [Error Handling](#error-handling)
  * [Server Selection](#server-selection)
  * [Debugging](#debugging)
* [Development](#development)
  * [Maturity](#maturity)
  * [Contributions](#contributions)

<!-- End Table of Contents [toc] -->

<!-- Start SDK Installation [installation] -->
## SDK Installation

### Getting started

JDK 11 or later is required.

The samples below show how a published SDK artifact is used:

Gradle:
```groovy
implementation 'pl.postivo:client:0.0.9'
```

Maven:
```xml
<dependency>
    <groupId>pl.postivo</groupId>
    <artifactId>client</artifactId>
    <version>0.0.9</version>
</dependency>
```

### How to build
After cloning the git repository to your file system you can build the SDK artifact from source to the `build` directory by running `./gradlew build` on *nix systems or `gradlew.bat` on Windows systems.

If you wish to build from source and publish the SDK artifact to your local Maven repository (on your filesystem) then use the following command (after cloning the git repo locally):

On *nix:
```bash
./gradlew publishToMavenLocal -Pskip.signing
```
On Windows:
```bash
gradlew.bat publishToMavenLocal -Pskip.signing
```
<!-- End SDK Installation [installation] -->

<!-- Start SDK Example Usage [usage] -->
## SDK Example Usage

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
#### Asynchronous Call
An asynchronous SDK client is also available that returns a [`CompletableFuture<T>`][comp-fut]. See [Asynchronous Support](#asynchronous-support) for more details on async benefits and reactive library integration.
```java
package hello.world;

import java.util.concurrent.CompletableFuture;
import pl.postivo.sdk.AsyncClient;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.operations.async.GetAccountDetailsResponse;

public class Application {

    public static void main(String[] args) {

        AsyncClient sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build()
            .async();

        CompletableFuture<GetAccountDetailsResponse> resFut = sdk.accounts().get()
                .call();

        resFut.thenAccept(res -> {
            if (res.accountResponse().isPresent()) {
            // handle response
            }
        });
    }
}
```

[comp-fut]: https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html
<!-- End SDK Example Usage [usage] -->

<!-- Start Asynchronous Support [async-support] -->
## Asynchronous Support

The SDK provides comprehensive asynchronous support using Java's [`CompletableFuture<T>`][comp-fut] and [Reactive Streams `Publisher<T>`][reactive-streams] APIs. This design makes no assumptions about your choice of reactive toolkit, allowing seamless integration with any reactive library.

<details>
<summary>Why Use Async?</summary>

Asynchronous operations provide several key benefits:

- **Non-blocking I/O**: Your threads stay free for other work while operations are in flight
- **Better resource utilization**: Handle more concurrent operations with fewer threads
- **Improved scalability**: Build highly responsive applications that can handle thousands of concurrent requests
- **Reactive integration**: Works seamlessly with reactive streams and backpressure handling

</details>

<details>
<summary>Reactive Library Integration</summary>

The SDK returns [Reactive Streams `Publisher<T>`][reactive-streams] instances for operations dealing with streams involving multiple I/O interactions. We use Reactive Streams instead of JDK Flow API to provide broader compatibility with the reactive ecosystem, as most reactive libraries natively support Reactive Streams.

**Why Reactive Streams over JDK Flow?**
- **Broader ecosystem compatibility**: Most reactive libraries (Project Reactor, RxJava, Akka Streams, etc.) natively support Reactive Streams
- **Industry standard**: Reactive Streams is the de facto standard for reactive programming in Java
- **Better interoperability**: Seamless integration without additional adapters for most use cases

**Integration with Popular Libraries:**
- **Project Reactor**: Use `Flux.from(publisher)` to convert to Reactor types
- **RxJava**: Use `Flowable.fromPublisher(publisher)` for RxJava integration
- **Akka Streams**: Use `Source.fromPublisher(publisher)` for Akka Streams integration
- **Vert.x**: Use `ReadStream.fromPublisher(vertx, publisher)` for Vert.x reactive streams
- **Mutiny**: Use `Multi.createFrom().publisher(publisher)` for Quarkus Mutiny integration

**For JDK Flow API Integration:**
If you need JDK Flow API compatibility (e.g., for Quarkus/Mutiny 2), you can use adapters:
```java
// Convert Reactive Streams Publisher to Flow Publisher
Flow.Publisher<T> flowPublisher = FlowAdapters.toFlowPublisher(reactiveStreamsPublisher);

// Convert Flow Publisher to Reactive Streams Publisher
Publisher<T> reactiveStreamsPublisher = FlowAdapters.toPublisher(flowPublisher);
```

For standard single-response operations, the SDK returns `CompletableFuture<T>` for straightforward async execution.

</details>

<details>
<summary>Supported Operations</summary>

Async support is available for:

- **[Server-sent Events](#server-sent-event-streaming)**: Stream real-time events with Reactive Streams `Publisher<T>`
- **[JSONL Streaming](#jsonl-streaming)**: Process streaming JSON lines asynchronously
- **[Pagination](#pagination)**: Iterate through paginated results using `callAsPublisher()` and `callAsPublisherUnwrapped()`
- **[File Uploads](#file-uploads)**: Upload files asynchronously with progress tracking
- **[File Downloads](#file-downloads)**: Download files asynchronously with streaming support
- **[Standard Operations](#example)**: All regular API calls return `CompletableFuture<T>` for async execution

</details>

[comp-fut]: https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html
[reactive-streams]: https://www.reactive-streams.org/
<!-- End Asynchronous Support [async-support] -->

<!-- Start Authentication [security] -->
## Authentication

### Per-Client Security Schemes

This SDK supports the following security scheme globally:

| Name     | Type | Scheme      |
| -------- | ---- | ----------- |
| `bearer` | http | HTTP Bearer |

To authenticate with the API the `bearer` parameter must be set when initializing the SDK client instance. For example:
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.GetAccountDetailsResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        GetAccountDetailsResponse res = sdk.accounts().get()
                .call();

        if (res.accountResponse().isPresent()) {
            // handle response
        }
    }
}
```
<!-- End Authentication [security] -->

<!-- Start Available Resources and Operations [operations] -->
## Available Resources and Operations

<details open>
<summary>Available methods</summary>

### [accounts()](docs/sdks/accounts/README.md)

* [get](docs/sdks/accounts/README.md#get) - Retrieve account details
* [getSubaccount](docs/sdks/accounts/README.md#getsubaccount) - Get subaccount details

#### [addressBook().contacts()](docs/sdks/contacts/README.md)

* [list](docs/sdks/contacts/README.md#list) - List contacts
* [add](docs/sdks/contacts/README.md#add) - Add a new contact
* [get](docs/sdks/contacts/README.md#get) - Retrieve contact details
* [update](docs/sdks/contacts/README.md#update) - Update a contact
* [delete](docs/sdks/contacts/README.md#delete) - Delete a contact
* [removeFromGroup](docs/sdks/contacts/README.md#removefromgroup) - Remove a contact from a group
* [addToGroup](docs/sdks/contacts/README.md#addtogroup) - Add a contact to a group

#### [addressBook().contacts().byExtId()](docs/sdks/byextid/README.md)

* [get](docs/sdks/byextid/README.md#get) - Retrieve contact details by EXT_ID
* [update](docs/sdks/byextid/README.md#update) - Update a contact by EXT_ID
* [delete](docs/sdks/byextid/README.md#delete) - Delete a contact by EXT_ID
* [removeFromGroup](docs/sdks/byextid/README.md#removefromgroup) - Remove a contact from a group by EXT_ID
* [addToGroup](docs/sdks/byextid/README.md#addtogroup) - Add a contact to a group by EXT_ID

#### [addressBook().groups()](docs/sdks/groups/README.md)

* [list](docs/sdks/groups/README.md#list) - List groups
* [add](docs/sdks/groups/README.md#add) - Add a new group
* [get](docs/sdks/groups/README.md#get) - Retrieve group details
* [update](docs/sdks/groups/README.md#update) - Update a group
* [delete](docs/sdks/groups/README.md#delete) - Delete a group

### [common()](docs/sdks/common/README.md)

* [ping](docs/sdks/common/README.md#ping) - Check API availability and version

### [metadata()](docs/sdks/metadata/README.md)

* [list](docs/sdks/metadata/README.md#list) - List metadata
* [getPredefinedConfigs](docs/sdks/metadata/README.md#getpredefinedconfigs) - List predefined configs

### [senders()](docs/sdks/senders/README.md)

* [list](docs/sdks/senders/README.md#list) - List senders
* [add](docs/sdks/senders/README.md#add) - Add a new sender
* [delete](docs/sdks/senders/README.md#delete) - Delete a sender
* [verify](docs/sdks/senders/README.md#verify) - Verify sender

### [shipments()](docs/sdks/shipments/README.md)

* [status](docs/sdks/shipments/README.md#status) - Retrieve shipment details with status events
* [cancel](docs/sdks/shipments/README.md#cancel) - Cancel shipments
* [dispatch](docs/sdks/shipments/README.md#dispatch) - Dispatch a new shipment
* [documents](docs/sdks/shipments/README.md#documents) - Retrieve documents related to a shipment
* [price](docs/sdks/shipments/README.md#price) - Check the shipment price

</details>
<!-- End Available Resources and Operations [operations] -->

<!-- Start Retries [retries] -->
## Retries

Some of the endpoints in this SDK support retries. If you use the SDK without any configuration, it will fall back to the default retry strategy provided by the API. However, the default retry strategy can be overridden on a per-operation basis, or across the entire SDK.

To change the default retry strategy for a single API call, you can provide a `RetryConfig` object through the `retryConfig` builder method:
```java
package hello.world;

import java.lang.Exception;
import java.util.concurrent.TimeUnit;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.GetAccountDetailsResponse;
import pl.postivo.sdk.utils.BackoffStrategy;
import pl.postivo.sdk.utils.RetryConfig;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        GetAccountDetailsResponse res = sdk.accounts().get()
                .retryConfig(RetryConfig.builder()
                    .backoff(BackoffStrategy.builder()
                        .initialInterval(1L, TimeUnit.MILLISECONDS)
                        .maxInterval(50L, TimeUnit.MILLISECONDS)
                        .maxElapsedTime(1000L, TimeUnit.MILLISECONDS)
                        .baseFactor(1.1)
                        .jitterFactor(0.15)
                        .retryConnectError(false)
                        .build())
                    .build())
                .call();

        if (res.accountResponse().isPresent()) {
            // handle response
        }
    }
}
```

If you'd like to override the default retry strategy for all operations that support retries, you can provide a configuration at SDK initialization:
```java
package hello.world;

import java.lang.Exception;
import java.util.concurrent.TimeUnit;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.GetAccountDetailsResponse;
import pl.postivo.sdk.utils.BackoffStrategy;
import pl.postivo.sdk.utils.RetryConfig;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .retryConfig(RetryConfig.builder()
                    .backoff(BackoffStrategy.builder()
                        .initialInterval(1L, TimeUnit.MILLISECONDS)
                        .maxInterval(50L, TimeUnit.MILLISECONDS)
                        .maxElapsedTime(1000L, TimeUnit.MILLISECONDS)
                        .baseFactor(1.1)
                        .jitterFactor(0.15)
                        .retryConnectError(false)
                        .build())
                    .build())
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        GetAccountDetailsResponse res = sdk.accounts().get()
                .call();

        if (res.accountResponse().isPresent()) {
            // handle response
        }
    }
}
```
<!-- End Retries [retries] -->

<!-- Start Error Handling [errors] -->
## Error Handling

Handling errors in this SDK should largely match your expectations. All operations return a response object or raise an exception.

By default, an API error will throw a `models/errors/APIException` exception. When custom error responses are specified for an operation, the SDK may also throw their associated exception. You can refer to respective *Errors* tables in SDK docs for more details on possible exception types for each operation. For example, the `get` method throws the following exceptions:

| Error Type                  | Status Code   | Content Type             |
| --------------------------- | ------------- | ------------------------ |
| models/errors/ErrorResponse | 401, 403, 4XX | application/problem+json |
| models/errors/ErrorResponse | 5XX           | application/problem+json |

### Example

```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.GetAccountDetailsResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        GetAccountDetailsResponse res = sdk.accounts().get()
                .call();

        if (res.accountResponse().isPresent()) {
            // handle response
        }
    }
}
```
<!-- End Error Handling [errors] -->

<!-- Start Server Selection [server] -->
## Server Selection

### Select Server by Name

You can override the default server globally using the `.server(AvailableServers server)` builder method when initializing the SDK client instance. The selected server will then be used as the default on the operations that use it. This table lists the names associated with the available servers:

| Name      | Server                                   | Description           |
| --------- | ---------------------------------------- | --------------------- |
| `prod`    | `https://api.postivo.pl/rest/v1`         | Production system     |
| `sandbox` | `https://api.postivo.pl/rest-sandbox/v1` | Test system (SANDBOX) |

#### Example

```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.GetAccountDetailsResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .server(Client.AvailableServers.SANDBOX)
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        GetAccountDetailsResponse res = sdk.accounts().get()
                .call();

        if (res.accountResponse().isPresent()) {
            // handle response
        }
    }
}
```

### Override Server URL Per-Client

The default server can also be overridden globally using the `.serverURL(String serverUrl)` builder method when initializing the SDK client instance. For example:
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.GetAccountDetailsResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .serverURL("https://api.postivo.pl/rest/v1")
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        GetAccountDetailsResponse res = sdk.accounts().get()
                .call();

        if (res.accountResponse().isPresent()) {
            // handle response
        }
    }
}
```
<!-- End Server Selection [server] -->

<!-- Start Debugging [debug] -->
## Debugging

### Debug
You can setup your SDK to emit debug logs for SDK requests and responses.

For request and response logging (especially json bodies), call `enableHTTPDebugLogging(boolean)` on the SDK builder like so:
```java
SDK.builder()
    .enableHTTPDebugLogging(true)
    .build();
```
Example output:
```
Sending request: http://localhost:35123/bearer#global GET
Request headers: {Accept=[application/json], Authorization=[******], Client-Level-Header=[added by client], Idempotency-Key=[some-key], x-speakeasy-user-agent=[speakeasy-sdk/java 0.0.1 internal 0.1.0 org.openapis.openapi]}
Received response: (GET http://localhost:35123/bearer#global) 200
Response headers: {access-control-allow-credentials=[true], access-control-allow-origin=[*], connection=[keep-alive], content-length=[50], content-type=[application/json], date=[Wed, 09 Apr 2025 01:43:29 GMT], server=[gunicorn/19.9.0]}
Response body:
{
  "authenticated": true, 
  "token": "global"
}
```
__WARNING__: This should only used for temporary debugging purposes. Leaving this option on in a production system could expose credentials/secrets in logs. <i>Authorization</i> headers are redacted by default and there is the ability to specify redacted header names via `SpeakeasyHTTPClient.setRedactedHeaders`.

__NOTE__: This is a convenience method that calls `HTTPClient.enableDebugLogging()`. The `SpeakeasyHTTPClient` honors this setting. If you are using a custom HTTP client, it is up to the custom client to honor this setting.

Another option is to set the System property `-Djdk.httpclient.HttpClient.log=all`. However, this second option does not log bodies.
<!-- End Debugging [debug] -->

<!-- Placeholder for Future Speakeasy SDK Sections -->

# Development

## Maturity

This SDK is in beta, and there may be breaking changes between versions without a major version update. Therefore, we recommend pinning usage
to a specific package version. This way, you can install the same version each time without breaking changes unless you are intentionally
looking for the latest version.

## Contributions

While we value open-source contributions to this SDK, this library is generated programmatically. Any manual changes added to internal files will be overwritten on the next generation. 
We look forward to hearing your feedback. Feel free to open a PR or an issue with a proof of concept and we'll do our best to include it in a future release.