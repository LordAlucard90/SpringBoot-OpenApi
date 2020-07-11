# Security

## Content

- [Security Schema](#security-schema)
- [Basic Auth](#basic-auth)
- [JWT Bearer Token Auth](#jwt-bearer-token-auth)
- [Anonymous Auth](#anonymous-auth)
- [Full Example](#full-example)

---

## Security Schema

There are a lot of ways to get authenticated into an api.
The [Security Scheme](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.3.md#securitySchemeObject)
define the specific way of the api.

---

## Basic Auth

It is possible to simply add the basic auth as root authentication method in this way:
```yaml
security: 
  - BasicAuth: []
components:
  securitySchemes:
    BasicAuth:
      type: http
      scheme: basic
```

---

## JWT Bearer Token Auth

As the basic auth it is possible to define the JWT token authentication in this way:
```yaml
security: 
  - JwtAuthToken: []
components:
  securitySchemes:
    JwtAuthToken:
      type: http
      scheme: bearer
      bearerFormat: JWT
```

---

## Anonymous Auth

In order to specify an open endpoint without security it can be added the security field with an empty array:
```yaml
paths:
  /examples:
    get:
      security: []
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
      security: []
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
      security: []
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
security: 
  - BasicAuth: []
  - JwtAuthToken: []
components:
  securitySchemes:
    BasicAuth:
      type: http
      scheme: basic
    JwtAuthToken:
      type: http
      scheme: bearer
      bearerFormat: JWT
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
