# Parameters

## Content

- [Parameter Object](#parameter-object)
- [Query Parameters](#query-parameters)
- [Path Parameters](#path-parameters)
- [Full Example](#full-example)

---

## Parameter Object

The [Parameter Object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.3.md#parameterObject)
describes a single parameter of the request.

Its main properties are:
- name
- in: path, query, header or cookie
- description
- required
- deprecated

---

## Query Parameters

Examples for query parameter are the current page and the maximum results for page:
```yaml
get:
  parameters:
    - name: curPage
      in: query
      description: Current page number
      schema:
        type: integer
        format: int32
        default: 0
    - name: pageSize
      in: query
      description: Max page elements
      required: false
      schema:
        type: integer
        format: int32
        default: 25
``` 
Since some parameter can be reused, like in this case for the paging of different resources,
they can be added to the component:
```yaml
components:
  parameters:
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
```
And listed in the parameters:
```yaml
get:
  parameters:
    - $ref: "#/components/parameters/CurPageParam"
    - $ref: "#/components/parameters/PageSizeParam"
```

---

## Path Parameters

Path parameters are used to access, for example, to a specific element. The need to be defined always required,

An example is:
```yaml
paths:
  /example/{uuidExample}:
    get:
      parameters:
        - name: uuidExample
          in: path
          description: Example uuid
          required: true
          schema:
            type: string
            format: uuid
      description: Return a specific examples
      responses:
        '200':
          description: An examples.
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Example"
```
It is important that the variable name between curly brackets and the parameter name match.

Like the Query parameter it is possible to move the path parameters in the components:
```yaml
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
```
And reuse them:
```yaml
get:
  parameters:
    - $ref: "#/components/parameters/UuidExampleParam"
```

---

## Full Example

```yaml
openapi: 3.0.0
info:
  version: '1.0'
  title: 'Basic Example'
  description: 'Basic specification example'
servers: 
  - url: http://example.com/api
    description: My example server 
paths:
  /example:
    get:
      parameters:
        - $ref: "#/components/parameters/CurPageParam"
        - $ref: "#/components/parameters/PageSizeParam"
      description: Return a list of examples
      responses:
        '200':
          description: A list of examples.
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/PagedResponseExample"
  /example/{uuidExample}:
    get:
      parameters:
        - $ref: "#/components/parameters/UuidExampleParam"
      description: Return a specific examples
      responses:
        '200':
          description: An examples.
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Example"
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
        uuidExamle:
          type: string
          format: uuid
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
