# Components

## Content

- [Component Object](#component-object)
- [Reusability](#reusability)
- [Inheritance](#inheritance)
- [Full Example](#full-example)

---

## Component Object

A [Component Object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.3.md#componentsObject)
holds a reusable object in hte Api definition. 

It can be for example:
- a schema
- a response
- a parameter
- etc.

The component is defined in the components section and can be used with a `$ref` to the component, 
the ref can also a be placed in a different file or in a url.

---

## Reusability

It is possible to define a basic component in the component section:
```yaml
components:
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
```
the in the next components it is possible to refer to is with `$ref`:
```yaml
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
```
In this way it is possible to define the response more clearly:
```yaml
responses:
  '200':
    description: A list of examples.
    content:
      application/json:
        schema:
          $ref: "#/components/schemas/ExampleList"
```

---

## Inheritance

It is possible for one property inherit all the properties of another one using the property `allOf`.

Starting from a Paged response like this:
```yaml
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
It is possible to refer to it in the sub object and define other properties like the content:
```yaml
PagedResponseExample:
  type: object
  allOf:
    - $ref: "#/components/schemas/PagedResponse"
  properties:
    content:
      $ref: "#/components/schemas/ExampleList"
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
      description: Return a list of examples
      responses:
        '200':
          description: A list of examples.
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/PagedResponseExample"
components:
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
