# Intro

## Content

- [HTTP Methods](#http-methods)
- [HTTP Status Codes](#http-status-codes)
- [OpenApi](#openapi)
- [OpenApi 2.0 vs 3.0](#openapi-20-vs-30)

---

## HTTP Methods
OpenApi is used to document an API, the most common way to build an API to the web is RESTful method 
that uses HTTP requests with JSON (mostly) calls.

The HTTP request methods are:
- **OPTIONS** returns the HTTP methods allowed.
- **HEAD** retrieves the metadata information of a resource.
- **GET** retrieves a resource.
- **POST** creates a new resource.
- **PUT** updates a resource or creates it if it does not exist.
- **PATCH** partially updates a resource.
- **DELETE** deletes a resource.
- **TRACE** prints the request to check is it was changed by an intermediate server.
- **CONNECT** converts the requests to a transparent TCP/IP tunnel.

## HTTP Status Codes
Status codes are used to communicate the result of a HTTP request.
The status code classes are:
- **1xx** information
- **2xx** success
- **3xx** redirect
- **4xx** client error
- **5xx** server error

## OpenApi

OpenApi is a program language agnostic way to define the API of an application.

Its porpoise is to give a only point of truth on how an api works.

It is possible to find a specification example [here](https://github.com/OAI/OpenAPI-Specification/blob/master/examples/v3.0/petstore.yaml)
with the corresponding gui [here](https://petstore.swagger.io/).

It also provides an online editor to write and check an api specification [here](https://editor.swagger.io/).

## OpenApi 2.0 vs 3.0

From open OpenApi 2.0 to 3.0 has be done a big improve on specification on generify of the components
in order to make them more reusable.
