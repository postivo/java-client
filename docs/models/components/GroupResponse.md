# GroupResponse

Address Book group details returned by the API.


## Fields

| Field                               | Type                                | Required                            | Description                         | Example                             |
| ----------------------------------- | ----------------------------------- | ----------------------------------- | ----------------------------------- | ----------------------------------- |
| `name`                              | *String*                            | :heavy_check_mark:                  | Group name.                         | my-group                            |
| `description`                       | *JsonNullable\<String>*             | :heavy_minus_sign:                  | Optional group description.         | This is a group for school contacts |
| `id`                                | *long*                              | :heavy_check_mark:                  | Unique system-assigned group ID.    | 234                                 |