# Requests

## Content

- [Summery And Description](#summery-and-description)
- [Tags](#tags)
- [Operation Id](#operation-id)
- [Request Body](#request-body)
- [Response Headers](#response-headers)
- [Read Only Properties](#read-only-properties)
- [Delete](#delete)
- [Other Response Codes](#other-response-codes)
- [Callbacks](#callbacks)
- [Full Example](#full-example)

---

## Summery And Description

In order to improve the documentation there the `summary ` and `descriptin` properties. 
The first one is used to get a basic idea of what the endpoint doest, the second to write a more detailed explanation.
```yaml
paths:
  /v1/examples:
    get:
      summary: Get all the Examples
      description: Return a paged list of examples.
  /v1/examples/{uuidExample}:
    get:
      summary: Get an Examples
      description: Return a specific examples using its **uuid**.
```
In the description it is possible to use markdown to improve the documentation.

---

## Tags

By default, all the apis are listed one after the other. It is possible to group the different operations using tags.
```yaml
paths:
  /v1/examples:
    get:
      tags: 
        - Example
  /v1/examples/{uuidExample}:
    get:
      tags: 
        - Example
```
It is possible to add multiple tags to a single operation.

---

## Operation Id

When are used tools to automatically generate code starting from the OpenApi definition, 
it is necessary to specify for each operation a unique, case-sensitive, `operatioId`:
```yaml
paths:
  /v1/examples:
    get:
      operationId: getAllExamplesV1
  /v1/examples/{uuidExample}:
    get:
      operationId: getAnExampleByUuidV1
```
This information is not displayed in the documentation, but it is used to generate the code.

---

## Request Body

When the request is POST is needed to specify the body of the request:
```yaml
post:
  summary: New Example
  description: Create a new Example.
  tags: 
    - Example
  operationId: createExampleV1
  requestBody:
    required: true
    content:
      application/json:
        schema:
          $ref: "#/components/schemas/Example"
  responses:
    '201':
      description: Example created.
```

---

## Response Headers

It is possible to define the response headers in this way:
```yaml
post:
  responses:
    '201':
      description: Example created.
      headers:
        location:
          description: Location of the created Example
          schema:
            type: string
            format: uri
            example: https://example.com/examples/{generatedUuid}
```

---

## Read Only Properties

There are a lot of properties that can be assigned to a filed. 
When a resource is created usually the ID is generated by the server and not decided by the client.
For this reason it should be documented as readonly:
```yaml
Example:
  type: object
  properties:
    uuidExample:
      type: string
      format: uuid
      readOnly: true
```

---

## Resource Update

The resources are updated using the primary id in this way:
```yaml
put:
  summary: Update an Examples
  description: Update a specific examples using its **uuid**.
  tags: 
    - Example
  operationId: updateAnExampleByUuidV1
  parameters:
    - $ref: "#/components/parameters/UuidExampleParam"
  requestBody:
    required: true
    content:
      application/json:
        schema:
          $ref: "#/components/schemas/Example"
  responses:
    '204':
      description: Costumer Updated.
```

---

## Delete

The delete operation description is very similar to the previous one, except for the body that is not needed:
```yaml
delete:
  summary: Delete an Examples
  description: Delete a specific examples using its **uuid**.
  tags: 
    - Example
  operationId: deleteAnExampleByUuidV1
  parameters:
    - $ref: "#/components/parameters/UuidExampleParam"
  responses:
    '200':
      description: Costumer Deleted.
```

---

## Other Response Codes

It is important to list all, or at least the most important, error responses that an api produce:
```yaml
responses:
  '204':
    description: Costumer Updated.
  '400':
    description: Bad request.
  '404':
    description: Not Found.
  '409':
    description: Conflict.
```

---

## Callbacks

OpenApi give the possibility to specify [Callbacks](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.3.md#callbackObject),
like WebHooks that are returned:
```yaml
callbacks:
  webHookNestedExample:
    '${request.body#wekHookExampleUrl}':
      description: A WebHook for NestedExample.
      post:
        requestBody:
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/NestedExample"
        responses:
          '200':
            description: Nester Example Updated.
```
Is is also necessary to create a related `wekHookExampleUrl` filed in the response body:
```yaml
Example:
    wekHookExampleUrl:
      type: string
      format: uri
```

---

## Full Example

```yaml
info:
  version: '1.0'
  title: 'Basic Example'
  description: 'Basic specification example'
  termsOfService: http://example.com/terms/
  contact:
    name: API Support
    url: http://www.example.com/support
    email: support@example.com
  license:
    name: Apache 2.0
    url: https://www.apache.org/licenses/LICENSE-2.0.html
servers: 
  - url: http://example.com/api
    description: My example server 
paths:
  /examples:
    get:
      summary: Get all the Examples
      description: Return a paged list of examples.
      tags: 
        - Example
      operationId: getAllExamplesV1
      parameters:
        - $ref: "#/components/parameters/CurPageParam"
        - $ref: "#/components/parameters/PageSizeParam"
      responses:
        '200':
          description: A list of examples.
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/PagedResponseExample"
    post:
      summary: New Example
      description: Create a new Example.
      tags: 
        - Example
      operationId: createExampleV1
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Example"
      responses:
        '201':
          description: Example created.
          headers:
            location:
              description: Location of the created Example
              schema:
                type: string
                format: uri
                example: http://example.com/examples/{generatedUuid}
        '400':
          description: Bad request.
        '409':
          description: Conflict.
      callbacks:
        webHookNestedExample:
          '${request.body#wekHookExampleUrl}':
            description: A WebHook for NestedExample.
            post:
              requestBody:
                content:
                  application/json:
                    schema:
                      $ref: "#/components/schemas/NestedExample"
              responses:
                '200':
                  description: Nester Example Updated.
  /examples/{uuidExample}:
    get:
      summary: Get an Examples
      description: Return a specific examples using its **uuid**.
      tags: 
        - Example
      operationId: getAnExampleByUuidV1
      parameters:
        - $ref: "#/components/parameters/UuidExampleParam"
      responses:
        '200':
          description: An examples.
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Example"
        '404':
          description: Not Found.
    put:
      summary: Update an Examples
      description: Update a specific examples using its **uuid**.
      tags: 
        - Example
      operationId: updateAnExampleByUuidV1
      parameters:
        - $ref: "#/components/parameters/UuidExampleParam"
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Example"
      responses:
        '204':
          description: Costumer Updated.
        '400':
          description: Bad request.
        '404':
          description: Not Found.
    delete:
      summary: Delete an Examples
      description: Delete a specific examples using its **uuid**.
      tags: 
        - Example
      operationId: deleteAnExampleByUuidV1
      parameters:
        - $ref: "#/components/parameters/UuidExampleParam"
      responses:
        '200':
          description: Costumer Deleted.
        '404':
          description: Not Found.
components:
  parameters:
    UuidExampleParam:
      name: uuidExample
      in: path
      description: Example uuid
      required: true
      schema:
        type: string
        format: uuid
    CurPageParam:
      name: curPage
      in: query
      description: Current page number
      schema:
        type: number
        format: int32
        default: 0
    PageSizeParam:
      name: pageSize
      in: query
      description: Max page elements
      required: false
      schema:
        type: number
        format: int32
        default: 25
  schemas:
    NestedExample:
      type: object
      properties:
        dateExample:
          type: string
          format: date
          example: '2012-12-12'
        booleanExample:
          type: boolean
          example: true
    Example:
      type: object
      properties:
        uuidExample:
          type: string
          format: uuid
          readOnly: true
        titleExample: 
          type: string
          example: My Example
        numberExample:
          type: number
          format: float
          example: 4.20
        emailExample:
          type: string
          format: email
        stateEnum:
          type: string
          enum: 
            - open
            - in progress
            - closed
          example: open
        colorEnum:
          type: string
          enum: [red, green, blue]
          example: red
        nestedExample:
          $ref: "#/components/schemas/NestedExample"
        wekHookExampleUrl:
          type: string
          format: uri
    ExampleList:
      type: array
      minLength: 1
      maxLength: 20
      items:
        $ref: "#/components/schemas/Example"
    PagedResponseExample:
      type: object
      allOf:
        - $ref: "#/components/schemas/PagedResponse"
      properties:
        content:
          $ref: "#/components/schemas/ExampleList"
    PagedResponse:
      type: object
      properties:
        curPage:
          type: number
          format: int32
          example: 0
        totalPages:
          type: number
          format: int32
          example: 10
        elementsForPage:
          type: number
          format: int32
          example: 25
```
