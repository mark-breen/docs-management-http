# Content types

## Get a content type

Gets an existing content type by the content type Id.

<span class="label label--get">GET</span> /api/management/projects/**{projectId}**/contenttypes/**{contentTypeId}**

### Parameters

| Name | Parameter type | Type | Format | Description |
|:-|:-|:-|:-|:-|
| projectId | path | string |  | The project identifier, e.g. "movieDb". Found in the project overview screen of the management console. |
| contentTypeId | path | string |  | The content type identifier. |
| versionStatus | query | string |  | The version status, either *published* or *latest*. The default is *latest*. |
| version | query | string | [versionNo](/model/version.md#versionNo)  | The specific version requested. |

### Example request

```http
GET: /api/management/projects/movieDb/contenttypes/movie/
```

### Response message

| HTTP status code | Reason | Response model |
|:-|:-|:-|
| 200 | Success | [Content Type](/model/content-type.md) |
| 401 | Unauthorized | [Error](/key-concepts/errors.md) |
| 404 | NotFound | [Error](/key-concepts/errors.md) |
| 500 | InternalServerError | [Error](/key-concepts/errors.md) |

### Remarks

If a specific *versionNo* value has been provided then the *versionStatus* value will be ignored.




## Create a content type

Creates a new content type resource.

<span class="label label--post">POST</span> /api/management/projects/**{projectId}**/contenttypes/

### Parameters

| Name | Parameter type | Type | Format | Description |
|:-|:-|:-|:-|:-|
| projectId | path | string |  | The project identifier, e.g. "movieDb". Found in the project overview screen of the management console. |

### Example request

```json
POST: /api/management/projects/movieDb/contenttypes/

{
    "id": "movie",
    "projectId": "movieDb",
    "name": {
        "en-GB": "Movie"
    },
    "description": {
        "en-GB": "A movie type"
    },
    "entryTitleField": "title",
    "entryDescriptionField": "overview",
    "fields": [
        {
            "id": "title",
            "name": {
                "en-GB": "Title"
            },
            "dataType": "string",
            "editor": {
                "id": "text",
                "instructions": {
                    "en-GB": "The title of the movie"
                },
                "properties": {
                    "placeholderText": {
                        "en-GB": "Enter the full title of the movie appropriate to the region"
                    }
                }
            }
        },
        {
            "id": "tagline",
            "name": {
                "en-GB": "Tagline"
            },
            "dataType": "string",
        },
        {
            "id": "overview",
            "name": {
                "en-GB": "Overview"
            },
            "dataType": "string",
            "dataFormat": "html"
        },
        {
            "id": "releaseDate",
            "name": {
                "en-GB": "Release Date"
            },
            "dataType": "dateTime",
            "validations": null
        },
        {
            "id": "actors",
            "name": {
                "en-GB": "Actors"
            },
            "dataType": "objectArray",
            "dataFormat": "entry",
            "validations": {
                "contentType": {
                    "contentType": "actor"
                }
            }
        }
    ],
    "defaultLanguage": "en-GB",
    "supportedLanguages": [
        "en-GB",
        "fr-FR",
        "de-DE",
        "es"
    ],
    "workflowId": "ContensisMultilingual",
    "dataFormat": "entry"
}
```

### Response message

| HTTP status code | Reason | Response model |
|:-|:-|:-|
| 201 | Created | [Content Type](/model/content-type.md) |
| 401 | Unauthorized | [Error](/key-concepts/errors.md) |
| 404 | NotFound | [Error](/key-concepts/errors.md) |
| 409 | Conflict | [Error](/key-concepts/errors.md) |
| 422 | ValidationError | [Error](/key-concepts/errors.md) |
| 500 | InternalServerError | [Error](/key-concepts/errors.md) |
**TODO: Add validation responses**

### Validations

| Type | Description |
|-|-|
| Project does not exist | A project must exist to be able to create content types. |
| Non-unique id | The content type must be unique for the project. |

### Remarks

If the *defaultLanguage* value is not included in the *supportedLanguages* array then it will automatically added by the service when the project is created. Additionally, if the *defaultLanguage* is not specified then the [project](/model/project.md) *primaryLanguage* will be automatically set as the value by the service.




## Update a content type

Creates a new content type resource.

<span class="label label--put">PUT</span> /api/management/projects/**{projectId}**/contenttypes/**{contentTypeId}**

### Parameters

| Name | Parameter type | Type | Format | Description |
|:-|:-|:-|:-|:-|
| projectId | path | string |  | The project identifier, e.g. "movieDb". Found in the project overview screen of the management console. |
| contentTypeId | path | string |  | The content type identifier. |

### Example request

```json
PUT: /api/management/projects/movieDb/contenttypes/movie

{
    "id": "movie",
    "projectId": "movieDb",
    "name": {
        "en-GB": "Movie"
    },
    "description": {
        "en-GB": "A movie type"
    },
    "entryTitleField": "title",
    "entryDescriptionField": "overview",
    "fields": [
        {
            "id": "title",
            "name": {
                "en-GB": "Title"
            },
            "dataType": "string",
            "editor": {
                "id": "text",
                "instructions": {
                    "en-GB": "The title of the movie"
                },
                "properties": {
                    "placeholderText": {
                        "en-GB": "Enter the full title of the movie appropriate to the region"
                    }
                }
            }
        },
        {
            "id": "tagline",
            "name": {
                "en-GB": "Tagline"
            },
            "dataType": "string"
        },
        {
            "id": "overview",
            "name": {
                "en-GB": "Overview"
            },
            "dataType": "string",
            "dataFormat": "html",
        },
        {
            "id": "releaseDate",
            "name": {
                "en-GB": "Release Date"
            },
            "dataType": "dateTime",
            "validations": null
        },
        {
            "id": "actors",
            "name": {
                "en-GB": "Actors"
            },
            "dataType": "objectArray",
            "dataFormat": "entry",
            "validations": {
                "contentType": {
                    "contentType": "actor"
                }
            }
        }
    ],
    "defaultLanguage": "en-GB",
    "supportedLanguages": [
        "en-GB",
        "fr-FR",
        "de-DE",
        "es"
    ],
    "workflowId": "ContensisMultilingual",
    "dataFormat": "entry",
    "version": {
        "versionNo": "2.0"
    }
}
```

### Response message

| HTTP status code | Reason | Response model |
|:-|:-|:-|
| 200 | Updated | [Content Type](/model/content-type.md) |
| 401 | Unauthorized | [Error](/key-concepts/errors.md) |
| 404 | NotFound | [Error](/key-concepts/errors.md) |
| 409 | NotLatestVersion | [Error](/key-concepts/errors.md) |
| 422 | ValidationError | [Error](/key-concepts/errors.md) |
| 500 | InternalServerError | [Error](/key-concepts/errors.md) |

### Validations

| Type | Description |
|-|-|
| Id does not exist | The content type must be unique for the project |
| Project does not exist | The content type must be unique for the project |

### Remarks

If the *defaultLanguage* value is not included in the *supportedLanguages* array then it will automatically added by the service when the project is created. Additionally, if the *defaultLanguage* is not specified then the [project](/model/project.md) *primaryLanguage* will be automatically set as the value by the service.




## Publish a content type

Publishes a content type making it available for creating entries.

<span class="label label--get">PUT</span> /api/management/workflow

## Parameters

None.

### Example request

```json
PUT: /api/management/workflow

{  
   "projectId":"movieDb",
   "objectId":"movie",
   "objectVersion":"0.4",
   "type":"ContentType",
   "workflowName":"ContensisDefault",
   "events":["submit","approve"]
}

```

### Response message

| HTTP status code | Reason | Response model |
|:-|:-|:-|
| 200 | Success | [Content Type [...]](/model/content-type.md) |
| 401 | Unauthorized | [Error](/key-concepts/errors.md) |
| 403 | BadRequest | [Error](/key-concepts/errors.md) |
| 404 | NotFound | [Error](/key-concepts/errors.md) |
| 500 | InternalServerError | [Error](/key-concepts/errors.md) |


## List content types

Gets the content types for a project.

<span class="label label--get">GET</span> /api/management/projects/**{projectId}**/contenttypes/

## Parameters

| Name | Parameter type | Type | Format | Description |
|:-|:-|:-|:-|:-|
| projectId | path | string |  | The project identifier, e.g. "movieDb". Found in the project overview screen of the management console. |
| versionStatus | query | string |  | The version status, either *published* or *latest*. The default is *latest*. |
| dataFormat | query | string |  | The specific dataFormat, e.g. *entry*, *asset*. The default is all. |

### Example request

```http
GET: /api/management/projects/movieDb/contenttypes?versionStatus=published&dataFormat=entry
```

### Response message

| HTTP status code | Reason | Response model |
|:-|:-|:-|
| 200 | Success | [Content Type [...]](/model/content-type.md) |
| 401 | Unauthorized | [Error](/key-concepts/errors.md) |
| 404 | NotFound | [Error](/key-concepts/errors.md) |
| 500 | InternalServerError | [Error](/key-concepts/errors.md) |




## Delete a content type

Deletes a content type within a specific project.

<span class="label label--delete">DELETE</span> /api/management/projects/**{projectId}**/contenttypes/**{contentTypeId}**

### Example request

```http
DELETE: /api/management/projects/movieDb/contenttypes/actor
```

### Response message

| HTTP status code | Reason | Response model |
|:-|:-|:-|
| 200 | Success |  |
| 401 | Unauthorized | [Error](/key-concepts/errors.md) |
| 404 | NotFound | [Error](/key-concepts/errors.md) |
| 500 | InternalServerError | [Error](/key-concepts/errors.md) |



## Add a component to a content type

Components are added to a content type as a field just like any other field, however the dataFormat of the field specifies the id of the component to add.

For example if you have a component with an id of "movieRole" then the dataFormat value would be "component.movieRole".

### Example JSON

```json
"fields":[
    {  
        "id":"movieRole",
        "name": {  
            "en-GB":"Movie Role"
        },
        "dataType":"object",
        "dataFormat":"component.movieRole",
        "description": {  
            "en-GB":"The movie Role"
        }
    }
]
```

After a content type has been saved with a component field, all of the component's fields are added to a nested fields collection within the component field on the content type. A components' fields are read-only, and cannot be modified from the content type.