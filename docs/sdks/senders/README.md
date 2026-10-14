# Senders

## Overview

### Available Operations

* [list](#list) - List senders
* [add](#add) - Add a new sender
* [delete](#delete) - Delete a sender
* [verify](#verify) - Verify sender

## list

Retrieve the senders defined in your account. Active, verified senders can be used for shipments. Inactive senders are also included in the list.

### Example Usage

<!-- UsageSnippet language="java" operationID="listSenders" method="get" path="/senders" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.ListSendersResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        ListSendersResponse res = sdk.senders().list()
                .call();

        if (res.senderDetails().isPresent()) {
            System.out.println(res.senderDetails().get());
        }
    }
}
```

### Response

**[ListSendersResponse](../../models/operations/ListSendersResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 4XX               | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## add

Create a sender using its address details. Additional senders are verified by mailing a verification code to the sender's address. Complete verification with the received code to activate the sender.

The sender can be created before the verification letter is sent. If sending the letter fails, retrieve the sender list before retrying creation.

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

    public static void main(String[] args) throws ErrorResponse, Exception {

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
            System.out.println(res.senderDetails().get());
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
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 409, 415, 4XX     | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## delete

Remove a sender using its system ID. The account's default sender must be retained.

### Example Usage

<!-- UsageSnippet language="java" operationID="deleteSender" method="delete" path="/senders/{id}" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.DeleteSenderResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        DeleteSenderResponse res = sdk.senders().delete()
                .id(14L)
                .call();

        // handle response
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
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 404, 409, 4XX     | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## verify

Activate a sender using the verification code received by mail. The code must match the current verification request, and the sender must still be awaiting verification.

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

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        VerifySenderResponse res = sdk.senders().verify()
                .id(443L)
                .requestBody(VerifySenderRequestBody.builder()
                    .verificationCode("A345FP73")
                    .build())
                .call();

        // handle response
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

| Error Type                   | Status Code                  | Content Type                 |
| ---------------------------- | ---------------------------- | ---------------------------- |
| models/errors/ErrorResponse  | 401, 405                     | application/problem+json     |
| models/errors/ErrorResponse  | 400, 403, 404, 409, 415, 4XX | application/problem+json     |
| models/errors/ErrorResponse  | 5XX                          | application/problem+json     |