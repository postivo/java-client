# AddressBook.Groups

## Overview

### Available Operations

* [list](#list) - List groups
* [add](#add) - Add a new group
* [get](#get) - Retrieve group details
* [update](#update) - Update a group
* [delete](#delete) - Delete a group

## list

Retrieve all contact groups defined in your account's Address Book. An Address Book without groups returns an empty list.

### Example Usage

<!-- UsageSnippet language="java" operationID="listGroups" method="get" path="/groups" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.ListGroupsResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        ListGroupsResponse res = sdk.addressBook().groups().list()
                .call();

        if (res.groupResponses().isPresent()) {
            System.out.println(res.groupResponses().get());
        }
    }
}
```

### Response

**[ListGroupsResponse](../../models/operations/ListGroupsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 4XX               | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## add

Create a new contact group in your account's Address Book.

### Example Usage

<!-- UsageSnippet language="java" operationID="addGroup" method="post" path="/groups" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.components.Group;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.AddGroupResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        Group req = Group.builder()
                .name("my-group")
                .description("This is a group for school contacts")
                .build();

        AddGroupResponse res = sdk.addressBook().groups().add()
                .request(req)
                .call();

        if (res.groupResponse().isPresent()) {
            System.out.println(res.groupResponse().get());
        }
    }
}
```

### Parameters

| Parameter                                  | Type                                       | Required                                   | Description                                |
| ------------------------------------------ | ------------------------------------------ | ------------------------------------------ | ------------------------------------------ |
| `request`                                  | [Group](../../models/shared/Group.md)      | :heavy_check_mark:                         | The request object to use for the request. |

### Response

**[AddGroupResponse](../../models/operations/AddGroupResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 415, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## get

Get the details of an Address Book group using the system ID assigned when the group was created.

### Example Usage

<!-- UsageSnippet language="java" operationID="getGroupById" method="get" path="/groups/{id}" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.GetGroupByIdResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        GetGroupByIdResponse res = sdk.addressBook().groups().get()
                .id(194L)
                .call();

        if (res.groupResponse().isPresent()) {
            System.out.println(res.groupResponse().get());
        }
    }
}
```

### Parameters

| Parameter          | Type               | Required           | Description        | Example            |
| ------------------ | ------------------ | ------------------ | ------------------ | ------------------ |
| `id`               | *long*             | :heavy_check_mark: | Group id to fetch  | 194                |

### Response

**[GetGroupByIdResponse](../../models/operations/GetGroupByIdResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 404, 4XX               | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## update

Update an Address Book group using its system ID. Submitting unchanged details leaves the group unchanged.

### Example Usage

<!-- UsageSnippet language="java" operationID="updateGroup" method="put" path="/groups/{id}" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.components.Group;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.UpdateGroupResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        UpdateGroupResponse res = sdk.addressBook().groups().update()
                .id(14L)
                .group(Group.builder()
                    .name("my-group")
                    .description("This is a group for school contacts")
                    .build())
                .call();

        if (res.groupResponse().isPresent()) {
            System.out.println(res.groupResponse().get());
        }
    }
}
```

### Parameters

| Parameter                                 | Type                                      | Required                                  | Description                               | Example                                   |
| ----------------------------------------- | ----------------------------------------- | ----------------------------------------- | ----------------------------------------- | ----------------------------------------- |
| `id`                                      | *long*                                    | :heavy_check_mark:                        | Group `id` to update.                     | 14                                        |
| `group`                                   | [Group](../../models/components/Group.md) | :heavy_check_mark:                        | Updated contact group details.            |                                           |

### Response

**[UpdateGroupResponse](../../models/operations/UpdateGroupResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 404, 415, 4XX     | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## delete

Remove an Address Book group using its system ID. By default, its contacts remain in the Address Book and their membership in the removed group is cleared. You can also choose to delete the contacts belonging to the group.

### Example Usage

<!-- UsageSnippet language="java" operationID="deleteGroup" method="delete" path="/groups/{id}" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.ContactHandling;
import pl.postivo.sdk.models.operations.DeleteGroupResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        DeleteGroupResponse res = sdk.addressBook().groups().delete()
                .id(876L)
                .contacts(ContactHandling.DETACH)
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                                                                | Type                                                                     | Required                                                                 | Description                                                              | Example                                                                  |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------ | ------------------------------------------------------------------------ | ------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| `id`                                                                     | *long*                                                                   | :heavy_check_mark:                                                       | Group `id` to remove.                                                    | 876                                                                      |
| `contacts`                                                               | [Optional\<ContactHandling>](../../models/operations/ContactHandling.md) | :heavy_minus_sign:                                                       | How to handle contacts that belong to the group.                         |                                                                          |

### Response

**[DeleteGroupResponse](../../models/operations/DeleteGroupResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 401, 405                    | application/problem+json    |
| models/errors/ErrorResponse | 400, 403, 404, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |