# Accounts
(*accounts()*)

## Overview

### Available Operations

* [get](#get) - Retrieve account details
* [getSubaccount](#getsubaccount) - Get subaccount details

## get

Retrieve the current account balance and other account details. You can also check the account limit and whether the account is a **main** account. Main accounts have unrestricted privileges and, via the [User Panel](https://panel.postivo.pl), you can create as many subaccounts as needed.

### Example Usage

<!-- UsageSnippet language="java" operationID="getAccountDetails" method="get" path="/account" -->
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

### Response

**[GetAccountDetailsResponse](../../models/operations/GetAccountDetailsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 403, 4XX               | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## getSubaccount

Check the account balance and other details, such as the subcredit balance of a subaccount. Subaccounts are additional users who can access your account’s services and data. You can restrict access levels and set privileges for subaccounts in the [User Panel](https://panel.postivo.pl).

Provide the full subaccount login to access its data.

### Example Usage

<!-- UsageSnippet language="java" operationID="getSubaccountDetails" method="get" path="/account/{user_login}" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.GetSubaccountDetailsResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        GetSubaccountDetailsResponse res = sdk.accounts().getSubaccount()
                .userLogin("some-login")
                .call();

        if (res.accountResponse().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                  | Type                                                       | Required                                                   | Description                                                | Example                                                    |
| ---------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------- |
| `userLogin`                                                | *String*                                                   | :heavy_check_mark:                                         | Login of the subaccount (user) for which to retrieve data. | some-login                                                 |

### Response

**[GetSubaccountDetailsResponse](../../models/operations/GetSubaccountDetailsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 403, 404, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |