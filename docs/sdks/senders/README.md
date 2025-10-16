# Senders
(*senders()*)

## Overview

### Available Operations

* [list](#list) - List senders
* [add](#add) - Add a new sender
* [delete](#delete) - Delete a sender
* [verify](#verify) - Verify sender

## list

Retrieve the list of allowed senders defined in your account. Senders are registered in your account and must be verified and activated before use. Activated senders have `active: true` property and can be used to send shipments. Inactive senders are also returned (`active: false`), but cannot be used until activated.

### Example Usage

<!-- UsageSnippet language="java" operationID="listSenders" method="get" path="/senders" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.ListSendersResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        ListSendersResponse res = sdk.senders().list()
                .call();

        if (res.senderDetails().isPresent()) {
            // handle response
        }
    }
}
```

### Response

**[ListSendersResponse](../../models/operations/ListSendersResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 400, 401, 403, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## add

Create a new sender on your account. The request must contain a `Sender` object. To prevent fraud, all additional senders are verified by mailing a verification code to the sender’s address. Complete the verification using the `verifySender` method. Verified senders have `active: true` and can be used to send shipments. Inactive senders are also returned (`active: false`), but cannot be used until verification is completed.

### Example Usage

<!-- UsageSnippet language="java" operationID="addSender" method="post" path="/senders" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.components.Sender;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.AddSenderResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        Sender req = Sender.builder()
                .name("Jan Nowak")
                .address("ul. Aleje Jerozolimskie")
                .postCode("00-999")
                .city("Warszawa")
                .name2("Firma Testowa Sp. z o.o.")
                .homeNumber("31")
                .flatNumber("2")
                .country("PL")
                .build();

        AddSenderResponse res = sdk.senders().add()
                .request(req)
                .call();

        if (res.senderDetails().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                  | Type                                       | Required                                   | Description                                |
| ------------------------------------------ | ------------------------------------------ | ------------------------------------------ | ------------------------------------------ |
| `request`                                  | [Sender](../../models/shared/Sender.md)    | :heavy_check_mark:                         | The request object to use for the request. |

### Response

**[AddSenderResponse](../../models/operations/AddSenderResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 400, 401, 403, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## delete

Remove a sender from your account by `id`. Pass the sender’s `id` parameter to remove it. The sender is deleted immediately.

### Example Usage

<!-- UsageSnippet language="java" operationID="deleteSender" method="delete" path="/senders/{id}" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.DeleteSenderResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        DeleteSenderResponse res = sdk.senders().delete()
                .id(14L)
                .call();

        if (res.errorResponse().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter              | Type                   | Required               | Description            | Example                |
| ---------------------- | ---------------------- | ---------------------- | ---------------------- | ---------------------- |
| `id`                   | *long*                 | :heavy_check_mark:     | Sender `id` to remove. | 14                     |

### Response

**[DeleteSenderResponse](../../models/operations/DeleteSenderResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 400, 401, 403, 404, 4XX     | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## verify

Verify a sender to activate it. After adding a new sender, a letter containing a verification code is mailed to the sender’s address. Provide this code to complete verification.

### Example Usage

<!-- UsageSnippet language="java" operationID="verifySender" method="patch" path="/senders/{id}" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.VerifySenderRequestBody;
import pl.postivo.sdk.models.operations.VerifySenderResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        VerifySenderResponse res = sdk.senders().verify()
                .id(443L)
                .requestBody(VerifySenderRequestBody.builder()
                    .verificationCode("A345FP73")
                    .build())
                .call();

        if (res.errorResponse().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                     | Type                                                                          | Required                                                                      | Description                                                                   | Example                                                                       |
| ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| `id`                                                                          | *long*                                                                        | :heavy_check_mark:                                                            | ID of the sender to verify.                                                   | 443                                                                           |
| `requestBody`                                                                 | [VerifySenderRequestBody](../../models/operations/VerifySenderRequestBody.md) | :heavy_check_mark:                                                            | Verification code received in the letter.                                     |                                                                               |

### Response

**[VerifySenderResponse](../../models/operations/VerifySenderResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 404                         | application/json            |
| models/errors/ErrorResponse | 400, 401, 403, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |