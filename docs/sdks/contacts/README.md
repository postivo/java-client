# AddressBook.Contacts

## Overview

### Available Operations

* [list](#list) - List contacts
* [add](#add) - Add a new contact
* [get](#get) - Retrieve contact details
* [update](#update) - Update a contact
* [delete](#delete) - Delete a contact
* [removeFromGroup](#removefromgroup) - Remove a contact from a group
* [addToGroup](#addtogroup) - Add a contact to a group

## list

Retrieve a paginated list of contacts defined in your account's **Address Book**. An Address Book without contacts returns an empty list.

### Example Usage

<!-- UsageSnippet language="java" operationID="listContacts" method="get" path="/contacts" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.ListContactsResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        ListContactsResponse res = sdk.addressBook().contacts().list()
                .page(1L)
                .limit(10L)
                .call();

        if (res.contactResponses().isPresent()) {
            System.out.println(res.contactResponses().get());
        }
    }
}
```

### Parameters

| Parameter               | Type                    | Required                | Description             | Example                 |
| ----------------------- | ----------------------- | ----------------------- | ----------------------- | ----------------------- |
| `page`                  | *Optional\<Long>*       | :heavy_minus_sign:      | Page number of results  | 1                       |
| `limit`                 | *Optional\<Long>*       | :heavy_minus_sign:      | Results limit per page. | 10                      |

### Response

**[ListContactsResponse](../../models/operations/ListContactsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 4XX               | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## add

Create a new contact in your account's **Address Book**. An optional external (custom) ID must be unique within the Address Book.

### Example Usage

<!-- UsageSnippet language="java" operationID="addContact" method="post" path="/contacts" -->
```java
package hello.world;

import java.lang.Exception;
import java.util.List;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.components.Contact;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.AddContactResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        Contact req = Contact.builder()
                .name("Jan Nowak")
                .address("ul. Aleje Jerozolimskie")
                .postCode("00-999")
                .city("Warszawa")
                .name2("Firma Testowa Sp. z o.o.")
                .homeNumber("31")
                .flatNumber("2")
                .phoneNumber("+48999999999")
                .extId("my-contact-1")
                .groupIds(List.of(
                    13L,
                    534L))
                .build();

        AddContactResponse res = sdk.addressBook().contacts().add()
                .request(req)
                .call();

        if (res.contactResponse().isPresent()) {
            System.out.println(res.contactResponse().get());
        }
    }
}
```

### Parameters

| Parameter                                  | Type                                       | Required                                   | Description                                |
| ------------------------------------------ | ------------------------------------------ | ------------------------------------------ | ------------------------------------------ |
| `request`                                  | [Contact](../../models/shared/Contact.md)  | :heavy_check_mark:                         | The request object to use for the request. |

### Response

**[AddContactResponse](../../models/operations/AddContactResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 409, 415, 4XX     | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## get

Get the details of an Address Book contact using its system ID.

### Example Usage

<!-- UsageSnippet language="java" operationID="getContactById" method="get" path="/contacts/{id}" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.GetContactByIdResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        GetContactByIdResponse res = sdk.addressBook().contacts().get()
                .id(14L)
                .call();

        if (res.contactResponse().isPresent()) {
            System.out.println(res.contactResponse().get());
        }
    }
}
```

### Parameters

| Parameter                     | Type                          | Required                      | Description                   | Example                       |
| ----------------------------- | ----------------------------- | ----------------------------- | ----------------------------- | ----------------------------- |
| `id`                          | *long*                        | :heavy_check_mark:            | Global contact `id` to fetch. | 14                            |

### Response

**[GetContactByIdResponse](../../models/operations/GetContactByIdResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 404, 4XX               | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## update

Update an Address Book contact using its system ID. Submitting unchanged details leaves the contact unchanged.

### Example Usage

<!-- UsageSnippet language="java" operationID="updateContact" method="put" path="/contacts/{id}" -->
```java
package hello.world;

import java.lang.Exception;
import java.util.List;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.components.Contact;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.UpdateContactResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        UpdateContactResponse res = sdk.addressBook().contacts().update()
                .id(14L)
                .contact(Contact.builder()
                    .name("Jan Nowak")
                    .address("ul. Aleje Jerozolimskie")
                    .postCode("00-999")
                    .city("Warszawa")
                    .name2("Firma Testowa Sp. z o.o.")
                    .homeNumber("31")
                    .flatNumber("2")
                    .phoneNumber("+48999999999")
                    .extId("my-contact-1")
                    .groupIds(List.of(
                        13L,
                        534L))
                    .build())
                .call();

        if (res.contactResponse().isPresent()) {
            System.out.println(res.contactResponse().get());
        }
    }
}
```

### Parameters

| Parameter                                     | Type                                          | Required                                      | Description                                   | Example                                       |
| --------------------------------------------- | --------------------------------------------- | --------------------------------------------- | --------------------------------------------- | --------------------------------------------- |
| `id`                                          | *long*                                        | :heavy_check_mark:                            | ID of the contact to update.                  | 14                                            |
| `contact`                                     | [Contact](../../models/components/Contact.md) | :heavy_check_mark:                            | Updated contact details.                      |                                               |

### Response

**[UpdateContactResponse](../../models/operations/UpdateContactResponse.md)**

### Errors

| Error Type                   | Status Code                  | Content Type                 |
| ---------------------------- | ---------------------------- | ---------------------------- |
| models/errors/ErrorResponse  | 401, 405                     | application/problem+json     |
| models/errors/ErrorResponse  | 400, 403, 404, 409, 415, 4XX | application/problem+json     |
| models/errors/ErrorResponse  | 5XX                          | application/problem+json     |

## delete

Remove an Address Book contact using its system ID.

### Example Usage

<!-- UsageSnippet language="java" operationID="deleteContact" method="delete" path="/contacts/{id}" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.DeleteContactResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        DeleteContactResponse res = sdk.addressBook().contacts().delete()
                .id(14L)
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                      | Type                           | Required                       | Description                    | Example                        |
| ------------------------------ | ------------------------------ | ------------------------------ | ------------------------------ | ------------------------------ |
| `id`                           | *long*                         | :heavy_check_mark:             | Global contact `id` to remove. | 14                             |

### Response

**[DeleteContactResponse](../../models/operations/DeleteContactResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 404, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## removeFromGroup

Remove a contact from an Address Book group using the contact and group system IDs. The contact remains in the Address Book.

### Example Usage

<!-- UsageSnippet language="java" operationID="removeContactFromGroup" method="delete" path="/contacts/{id}/group/{group_id}" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.RemoveContactFromGroupResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        RemoveContactFromGroupResponse res = sdk.addressBook().contacts().removeFromGroup()
                .id(35L)
                .groupId(656L)
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                                     | Type                                          | Required                                      | Description                                   | Example                                       |
| --------------------------------------------- | --------------------------------------------- | --------------------------------------------- | --------------------------------------------- | --------------------------------------------- |
| `id`                                          | *long*                                        | :heavy_check_mark:                            | Global contact `id` to detach from the group. | 35                                            |
| `groupId`                                     | *long*                                        | :heavy_check_mark:                            | Group `id` to detach from the contact.        | 656                                           |

### Response

**[RemoveContactFromGroupResponse](../../models/operations/RemoveContactFromGroupResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 404, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## addToGroup

Assign a contact to an Address Book group using the contact and group system IDs. Both the contact and the group must exist in your Address Book.

### Example Usage

<!-- UsageSnippet language="java" operationID="addContactToGroup" method="patch" path="/contacts/{id}/group/{group_id}" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.AddContactToGroupResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        AddContactToGroupResponse res = sdk.addressBook().contacts().addToGroup()
                .id(35L)
                .groupId(656L)
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                                 | Type                                      | Required                                  | Description                               | Example                                   |
| ----------------------------------------- | ----------------------------------------- | ----------------------------------------- | ----------------------------------------- | ----------------------------------------- |
| `id`                                      | *long*                                    | :heavy_check_mark:                        | Global contact `id` to add to the group.  | 35                                        |
| `groupId`                                 | *long*                                    | :heavy_check_mark:                        | Group `id` to associate with the contact. | 656                                       |

### Response

**[AddContactToGroupResponse](../../models/operations/AddContactToGroupResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 404, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |