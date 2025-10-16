# Groups
(*addressBook().groups()*)

## Overview

### Available Operations

* [list](#list) - List groups
* [add](#add) - Add a new group
* [get](#get) - Retrieve group details
* [update](#update) - Update a group
* [delete](#delete) - Delete a group

## list

Retrieve the full list of groups defined in your account’s Address Book.

### Example Usage

<!-- UsageSnippet language="java" operationID="listGroups" method="get" path="/groups" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.ListGroupsResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        ListGroupsResponse res = sdk.addressBook().groups().list()
                .call();

        if (res.groupResponses().isPresent()) {
            // handle response
        }
    }
}
```

### Response

**[ListGroupsResponse](../../models/operations/ListGroupsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 400, 401, 403, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## add

Create a new contact group in your account’s Address Book.

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

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

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
            // handle response
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
| models/errors/ErrorResponse | 400, 401, 403, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## get

Get the details of a single group from your Address Book by its `id` (returned when the group was created).

### Example Usage

<!-- UsageSnippet language="java" operationID="getGroupById" method="get" path="/groups/{id}" -->
```java
package hello.world;

import java.lang.Exception;
import pl.postivo.sdk.Client;
import pl.postivo.sdk.models.errors.ErrorResponse;
import pl.postivo.sdk.models.operations.GetGroupByIdResponse;

public class Application {

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        GetGroupByIdResponse res = sdk.addressBook().groups().get()
                .id(194L)
                .call();

        if (res.groupResponse().isPresent()) {
            // handle response
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
| models/errors/ErrorResponse | 400, 401, 404, 4XX          | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## update

Update a group’s details by ID.

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

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

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
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                 | Type                                      | Required                                  | Description                               | Example                                   |
| ----------------------------------------- | ----------------------------------------- | ----------------------------------------- | ----------------------------------------- | ----------------------------------------- |
| `id`                                      | *long*                                    | :heavy_check_mark:                        | Group `id` to update.                     | 14                                        |
| `group`                                   | [Group](../../models/components/Group.md) | :heavy_check_mark:                        | A `Group` object with the updated fields. |                                           |

### Response

**[UpdateGroupResponse](../../models/operations/UpdateGroupResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/ErrorResponse | 400, 401, 403, 404, 4XX     | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |

## delete

Remove a group from your account’s Address Book by `ID`. Pass the group’s `id` to remove it. The group is deleted immediately from the Address Book.

If you also want to remove contacts that belong to this group, set the parameter `contacts` to `delete`. The default is `contacts: detach`, which detaches contacts from the removed group but leaves them in the Address Book.

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

    public static void main(String[] args) throws ErrorResponse, ErrorResponse, Exception {

        Client sdk = Client.builder()
                .bearer("<YOUR API ACCESS TOKEN>")
            .build();

        DeleteGroupResponse res = sdk.addressBook().groups().delete()
                .id(876L)
                .contacts(ContactHandling.DETACH)
                .call();

        if (res.errorResponse().isPresent()) {
            // handle response
        }
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
| models/errors/ErrorResponse | 400, 401, 403, 404, 4XX     | application/problem+json    |
| models/errors/ErrorResponse | 5XX                         | application/problem+json    |