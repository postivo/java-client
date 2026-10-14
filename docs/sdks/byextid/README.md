# AddressBook.Contacts.ByExtId

## Overview

### Available Operations

* [get](#get) - Retrieve contact details by EXT_ID
* [update](#update) - Update a contact by EXT_ID
* [delete](#delete) - Delete a contact by EXT_ID
* [removeFromGroup](#removefromgroup) - Remove a contact from a group by EXT_ID
* [addToGroup](#addtogroup) - Add a contact to a group by EXT_ID

## get

Get the details of an Address Book contact using its external (custom) ID, assigned when the contact was created.

### Example Usage

<!-- UsageSnippet language="java" operationID="getContactByExternalId" method="get" path="/contacts/external/{ext_id}" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.GetContactByExternalIdResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        GetContactByExternalIdResponse res = sdk.addressBook().contacts().byExtId().get()
                .extId("my-ext-id-44")
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
| `extId`                                       | *String*                                      | :heavy_check_mark:                            | External (custom) ID of the contact to fetch. | my-ext-id-44                                  |

### Response

**[GetContactByExternalIdResponse](../../models/operations/GetContactByExternalIdResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 404, 4XX               | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## update

Update an Address Book contact using its external (custom) ID, assigned when the contact was created. Submitting unchanged details leaves the contact unchanged.

### Example Usage

<!-- UsageSnippet language="java" operationID="updateContactByExternalId" method="put" path="/contacts/external/{ext_id}" -->
```java
package hello.world;

import java.lang.Exception;
import java.util.List;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.components.Contact;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.UpdateContactByExternalIdResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        UpdateContactByExternalIdResponse res = sdk.addressBook().contacts().byExtId().update()
                .extId("my-id-2")
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

| Parameter                                      | Type                                           | Required                                       | Description                                    | Example                                        |
| ---------------------------------------------- | ---------------------------------------------- | ---------------------------------------------- | ---------------------------------------------- | ---------------------------------------------- |
| `extId`                                        | *String*                                       | :heavy_check_mark:                             | External (custom) ID of the contact to update. | my-id-2                                        |
| `contact`                                      | [Contact](../../models/components/Contact.md)  | :heavy_check_mark:                             | Updated contact details.                       |                                                |

### Response

**[UpdateContactByExternalIdResponse](../../models/operations/UpdateContactByExternalIdResponse.md)**

### Errors

| Error Type                   | Status Code                  | Content Type                 |
| ---------------------------- | ---------------------------- | ---------------------------- |
| models/errors/ErrorResponse  | 401, 405                     | application/problem+json     |
| models/errors/ErrorResponse  | 400, 403, 404, 409, 415, 4XX | application/problem+json     |
| models/errors/ErrorResponse  | 5XX                          | application/problem+json     |

## delete

Remove an Address Book contact using its external (custom) ID, assigned when the contact was created.

### Example Usage

<!-- UsageSnippet language="java" operationID="deleteContactByExternalId" method="delete" path="/contacts/external/{ext_id}" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.DeleteContactByExternalIdResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        DeleteContactByExternalIdResponse res = sdk.addressBook().contacts().byExtId().delete()
                .extId("my-id-2")
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                                      | Type                                           | Required                                       | Description                                    | Example                                        |
| ---------------------------------------------- | ---------------------------------------------- | ---------------------------------------------- | ---------------------------------------------- | ---------------------------------------------- |
| `extId`                                        | *String*                                       | :heavy_check_mark:                             | External (custom) ID of the contact to remove. | my-id-2                                        |

### Response

**[DeleteContactByExternalIdResponse](../../models/operations/DeleteContactByExternalIdResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 404, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## removeFromGroup

Remove a contact from an Address Book group using the contact's external (custom) ID and the group's system ID. The contact remains in the Address Book.

### Example Usage

<!-- UsageSnippet language="java" operationID="removeContactByExtIdToGroup" method="delete" path="/contacts/external/{ext_id}/group/{group_id}" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.RemoveContactByExtIdToGroupResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        RemoveContactByExtIdToGroupResponse res = sdk.addressBook().contacts().byExtId().removeFromGroup()
                .extId("my-id-1")
                .groupId(656L)
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                                                     | Type                                                          | Required                                                      | Description                                                   | Example                                                       |
| ------------------------------------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------- |
| `extId`                                                       | *String*                                                      | :heavy_check_mark:                                            | External (custom) ID of the contact to detach from the group. | my-id-1                                                       |
| `groupId`                                                     | *long*                                                        | :heavy_check_mark:                                            | Group `id` to detach from the contact.                        | 656                                                           |

### Response

**[RemoveContactByExtIdToGroupResponse](../../models/operations/RemoveContactByExtIdToGroupResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 404, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## addToGroup

Assign a contact to an Address Book group using the contact's external (custom) ID and the group's system ID. Both the contact and the group must exist in your Address Book.

### Example Usage

<!-- UsageSnippet language="java" operationID="addContactByExtIdToGroup" method="patch" path="/contacts/external/{ext_id}/group/{group_id}" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.AddContactByExtIdToGroupResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        AddContactByExtIdToGroupResponse res = sdk.addressBook().contacts().byExtId().addToGroup()
                .extId("my-id-1")
                .groupId(656L)
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                                                | Type                                                     | Required                                                 | Description                                              | Example                                                  |
| -------------------------------------------------------- | -------------------------------------------------------- | -------------------------------------------------------- | -------------------------------------------------------- | -------------------------------------------------------- |
| `extId`                                                  | *String*                                                 | :heavy_check_mark:                                       | External (custom) ID of the contact to add to the group. | my-id-1                                                  |
| `groupId`                                                | *long*                                                   | :heavy_check_mark:                                       | Group `id` to associate with the contact.                | 656                                                      |

### Response

**[AddContactByExtIdToGroupResponse](../../models/operations/AddContactByExtIdToGroupResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 404, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |