# Stuff for free

*or*

## What I've learned about Swagger

---

# What is it?

> Swagger is a language-agnostic open specification to describe REST APIs.

Format is largely based on JSON Schema.

---

## Swagger is

...an open standard based on open standards.

...is a data structure to describe data structures.

...is a specification that specifies specifications.

---

![](img/yertle-turtle.jpg)

...turtles all the way down.

---

## RESTful

Covers most things important to an API consumer.

* host
* HTTP paths, + verbs
* parameters
* ACL/permissions
* security schemes
* response schemas

---

## Versioned

timeline

* _1.0_ - published 2011-08-10
* _1.1_ - published 2011-08-10
* _1.2_ - published 2014-03-14
* __2.0__ - published 2014-09-08
* _3.0_ - in progress


---

![](img/raging-hellfish.jpg)

_JS Platform supports v2.0_

---

## Spec formats

* `.json` (JSON Schema)
* `.yaml`

Convention is to name a swagger spec `swagger.json` (or `.yaml`)

---

## Now for some vocab

Let's all speak the same language

---

## Spec

_Swagger_ is a specification, not a library.

---v

Some libraries we use with swagger:
* `swagger-tools` - for Carbon API
* `swagger-ui` - for api documentation
* `swagger-client` - for `api-clients`

Note:
Often hear people talking about "Swagger". It can be very confusing to refer to
anything related to swagger as "in swagger" or "using swagger".

---

## "paths object"

This is where your routes, verbs, and parameters go.

Params shared across operations can be declared at the path level

---

## "operation":

_combination of an HTTP path + verb_


---v

`operationId` is used to uniquely identify path/verb combinations when
generating client clients

---

## "schema":

_a format for data_

---v

Lifted directly from JSON Schema and extended many places in swagger specs:

* data types
* parameters
* response objects

Note:
Similar to a model

---

## "parameters"

_data used by an operation_

---v

Describes data in:

* query params
* path via template matching
* headers
* body
* files

---

## "definitions"

_reusable bits of a spec_

---v

* definitions (abstract definitions)
* parameter definitions
* responses defintions
* security definitions

Improves readability, saves typing, and can even be reused across multiple specs!

---v

```
"Tag": {
    "type": "object",
    "properties": {
      "id": {
        "type": "integer",
        "format": "int64"
      },
      "name": {
        "type": "string"
      }
    }
  }
```

---

## "reference object"

_a link to a reusable definition_

Use these!

---v

Same file: `"$ref": "#/definition/searchTerm"`

Another file: `"$ref": "Search.json"`

A definition in another file: `"$ref": "Search.json#/definition/searchTerm"`


---

![](img/more-you-know.jpg)

---

## Some other useful stuff I learned...

---

Online editor is SUPER useful

editor.swagger.io

---

## Descriptions are not optional

---

![](img/dont-panic.jpg)

* often the only required field
* can be markdown

---

## Swagger specification can be extended

Many parts of the spec support vendor-specific extension and customization.

`x-` is the vendor-specific prefix.

---


## Authentication

Special headers. Defined with SecurityDefinitions, not parameters.

---

## Supported auth strategies

* oauth2
* security token
* user/pass

---

Requires both a security definition **and** a security object.

The names should match:

```
// top level
"securityDefinitions": {
  "contributor": { /* ... */ }
},

// in an operation or path
"security": [
  {
    "contributor": [
      "user.view" // no ACL? this can be empty
    ]
  }
],
```

---

## A well written spec?

* uses references
* has good text descriptions
* factors out common structures
* reuses definitions

---

## So why use it?

---

## Better documentation

Most developers first encounter swagger spec by interacting with a Swagger UI site.

But the swagger specification is so much more than that.

It gives us a way to stop repeating ourselves.

---

## Way more than just docs

Other tooling and libraries available for:

* application routing
* security + ACL middleware
* route parameter validation
* auto-generated API clients
* database model generation
* automated api testing
* type definitions
* application frameworks (hi carbon!)

---

## Why is it important?

---

## Garden of the commons

At its core, the swagger spec carves out a common ground for both computers
and humans to understand REST APIs. With these common terms, we can write and
reuse tools and libraries across multiple APIs.

When used to map routes to controllers and validate parameters, a swagger spec
becomes *the* canonical representation of an API.

Note:

Just like a REST API establishes a contract for interacting with an application,
a Swagger spec establishes a contract for interacting with APIs in general.

Within this common ground, anyone can craft and share tools and libraries that
work together, regardless of platform, language

---

## OpenAPI is the new Swagger

On January 1st, 2016 Swagger was been donated to the [Open API Initiative](https://openapis.org)
and renamed OpenAPI Specification.

---v

## In good company

The Open API Initiative is a Linux Foundation Collaboration project (just like Node!)

Some members you may have heard of:

* Google
* IBM
* Microsoft
* Atlassian
* CapitalOne
* Apiary
* and more...

---

Find internal specs at your friendly api, or on our github https://github.shuttercorp.net/web-platform/swagger-specs/
