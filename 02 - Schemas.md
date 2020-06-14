# Schemas

## Content

- [JSON Schema](#json-schema)
- [Data Types](#data-types)
- [Objects](#objects)
- [Enums](#enums)
- [Full Example](#full-example)

---

## JSON Schema

JSON is the most used format for requests and responses, 
it is possible to define and validate JSON data structure and formats
using [JSON Schema](https://json-schema.org/understanding-json-schema/).

---

## Data Types

There are different [Data Types](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.3.md#dataTypes)
supported in the OpenApi specification:
- integer
- number
- string
- boolean		
- date, date-time

All this base data types supports the [Schema Object Definition](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.3.md#schemaObject)
that allows the definition of filed specific properties like length or minimum value.

For example:
```yaml
application/json:
  schema:
    type: array
    minLength: 1
    maxLength: 20
    items:
      type: string
      description: Example String
      minLength: 2
      maxLength: 100
```

---

## Objects

It is possible also to define more complex objects using the type Object.

Inside the object it is possible to define fields that are automatically managed as schemas:
```yaml
items:
  type: object
  properties:
    uuidExamle:
      type: string
      format: uuid
    titleExample: 
      type: string
      example: My Example
    emailExample:
      type: string
      format: email
    numberExample:
      type: number
      format: float
      example: 4.20
    nestedExample:
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

The example field allows to have a more robust representation of the data.

---

## Enums

Enumerations are used to specify a fixed set of values that can be used for a field,
can be defined in two different ways:
```yaml
properties:
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
  /v1/examples:
    get:
      description: Return a list of examples
      responses:
        '200':
          description: A list of examples.
          content:
            application/json:
              schema:
                type: array
                minLength: 1
                maxLength: 20
                items:
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
