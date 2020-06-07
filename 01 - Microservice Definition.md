# Microservice Definition

## Content

- [Swagger Hub](#swagger-hub)
- [Specification](#specification)
- [Info Object](#info-object)
- [Server Object](#server-object)
- [Path Object](#path-object)
- [Full Example](#full-example)

---

## Swagger Hub

Swagger Hub is a tool that helps to develop APIs, it can be found [here](https://swagger.io/tools/swaggerhub/) and
can be created a free  14-days trial account logging with GitHub account.

It allows to edit the apis in one side and in the other to see the result. 
It also provides tools for navigation and definition.

The basic structure of a OpenApi yaml is:
```yaml
openapi: 3.0.0
info:
  version: '1.0'
  title: 'Basic Example'
  description: 'Basic specification example'
paths: {}
```

---

## Specification

In the [GitHub Page](https://github.com/OAI/OpenAPI-Specification) it is possible to find a lot onf information about open api.

For example in [Implementions](https://github.com/OAI/OpenAPI-Specification/blob/master/IMPLEMENTATIONS.md)
it is possible to find a lot of editors and user interfaces for many languages.

In the [Versions](https://github.com/OAI/OpenAPI-Specification/tree/master/versions) it is possible to find
all the specifications for all the different versions of Open Api.

---

## Info Object

The [Info Object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.3.md#infoObject) 
provides metadata about the Api, Title and Version are required, all the other information are optional.

An example structure from the Pet Store example is:
```yaml
info:
    title: Sample Pet Store App
    description: This is a sample server for a pet store.
    termsOfService: http://example.com/terms/
    contact:
      name: API Support
      url: http://www.example.com/support
      email: support@example.com
    license:
      name: Apache 2.0
      url: https://www.apache.org/licenses/LICENSE-2.0.html
    version: 1.0.1
```

---

## Server Object

The [Server Object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.3.md#serverObject) 
is used to specify where the api is available.

In this property only the Url is mandatory but also the description can be useful:
```yaml
servers: 
  - url: http://example.com/api
    description: My example server 
```

---

## Path Object

The [Path Object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.3.md#pathsObject)
provides the relative path to reach the endpoint operation.

With this object it is possible to specify all the request and response data with the corresponding return code:
```yaml
paths:
  /example:
    get:
      description: Return a list of examples
      responses:
        '200':
          description: A list of examples.
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
```
