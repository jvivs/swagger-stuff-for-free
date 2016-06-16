# Stuff for free

*or*

## What I've learned about Swagger so you don't have to

---

## *Prologue*

---

![](img/sstk-logo.jpg)

---v

![](img/dog-chameleon.jpg)

---v

![](img/watermelon-man.jpg)

---v

Our path to Swagger

---

![](img/perl-mason.jpg)

10 years of development on a Perl Mason monolith

---

Enabled rapid early iteration

---

Yesterday's convenience can be today's nightmare

---

![](img/the-monolith-monsters1.jpg)

Note:
Over time, the monolith becomes a monster.

---

## Monolithic problems

- poor separation of concerns
- DB calls the views
- hot spots hard to spot
- dynamic code reloading
- general cruft
- knowledge attrition

---

A monolith is all too often a victim of its own success

---

## Solution?

---

![](img/services-for-all-the-things.jpg)

Kill the monolith with services

---

## Consumer-facing stacks c. 2014

- **Perl**: Shutterstock.com
- **PHP**: Video, Premier, Bigstock.com
- **Node.js**: Offset.com, Music

---

## Early polyglot services

- Translations (Perl, 2011)
- Media (Perl, 2011)
- Search (Java, 2011)
- Accounts (Node.js, 2012)
- Commerce (Ruby, 2013)
- Remedia (Perl, 2013)
- Fastmedia (Node.js, 2014)
- Media v2 (Node.js, 2014)

---

What happened to the Monolith?

---v

Three years later we're still not done

---v

But we've gotten closer...

---v

![](img/nodejs-dark.png)

(that's why you're here)

---v

And we've learned a few things along the way.

---

## *Act 1: Services*

---

Great way to isolate change

---

Services have their own set of problems

---

Another kind of suffering?

---

## Discovery

# 🕵

---

"Where do I find the stuff?"

---v

Code

# 🙃

---v

Github orgs and repos

# 😨

---v

Confluence/wikis
#  🙄

---v

Jenkins

# 😳

---v

Deployment tools
# 🤖

---

`$\sum_{APIs}\ast\sum_{API\_Versions}\ast\sum_{API\_List\_Locations}$`

Finding the right service and version gets harder with scale

---

## Documentation

# 📖

---

- harder to find
- inconsistent structure
- incomplete content
- immediately stale
- language-specific terminology

---

Engineers are better at code than docs.

---

![](img/tribal-knowledge.jpg)

Tribal knowledge doesn't scale.

---

## Interface

names are hard

#🤔

---

Best-practices differ across languages.

---v

`ruby_likes_snake_case`

---v

```
{
  jsNaming_conventions: {
    {
      arguments: true
    }
  }
}
```

---v

`<Java xml="true" interop="mixed" size="huge" />`

---

And it gets weirder over time...

---v

`GET /just?throw=someStuff.in[here]&chill`

---v

```
{
  user_id: 1,
  userId: "1"
}
```

---v

```
HTTP/1.1 200 OK

{
  code: 500,
  message: 'not ok'
}
```

---

## API balkanization is real

Orchestrating even the most internally-consistent APIs can be painful when the
naming conventions don't line up.

---

## Orchestration via REST

##🎹🎹😴🎹🎹😴🎹🎹

---


## Raw HTTP clients

- inconsistent app implementation
- inconsistent async interfaces
- brittle
- complex
- DRY, what's that?

---

## Client libraries

handcrafted, bespoke

#🍸

---

Seems like a good idea at first.

---

- incomplete
- team/app-specific
- inconsistent patterns
- business-logic creep
- test coverage?
- bugs!

---

![](img/face-plant.jpg)

Don't forget, clients need documentation too.

---

Usability problems with services compound exponentially with scale

---

`$\sum_{APIs}\ast\sum_{API\_Versions}\ast\sum_{(Languages\ast Interfaces)}\ast\sum_{Clients}$`

Complexity growth curve of hand-written client libraries gets rough.

&Theta;(n<sup>4</sup>)

---

![](img/sky-is-falling.jpg)

...so is the sky falling?

---

## The sky is not falling

Service problem are an easier class of problems than monolith problems.

---

## Isolation

- less code = lower complexity
- fewer side effects
- less knowledge required

---

## *Act 2: Common ground*

![](img/venn-diagram-definitions.png)

(this is my most awesome diagram)

---

Swagger is an opportunity to simplify

---

## What is Swagger?

![](img/swagger.png)

---

## Swagger is

...a specification that specifies specifications.

...a data structure to describe data structures.

...an open standard based on open standards.

---

![](img/yertle-turtle.jpg)

...turtles all the way down.

---

## Commonly misunderstood

Swagger is not a tool itself, but a *specification* with tooling.

---

Technically speaking...

> Swagger is a language-agnostic open specification to describe REST APIs.

...it's a JSON Schema format.

---

## RESTful

Covers most things important to an API consumer

* host
* HTTP paths + verbs
* parameters
* security schemes
* ACL/roles
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

## Formats

* `.json` (JSON Schema)
* `.yaml`

Convention is to name a swagger spec `swagger.json` (or `.yaml`)

---

## Now for some vocab

Let's all speak the same language

---

## Spec

__Swagger__ is the specification.

A Swagger __spec__ describes an API

Note:
Often hear people talking about "Swagger". It can be very confusing to refer to
anything related to swagger as "in swagger" or "using swagger".

---

## "paths" object

This is where your routes, verbs, and parameters go

Note:
Think about this as your application router. With `swagger-tools` it can be.

---v

Params shared across operations can be declared at the path level

---

## "operation"

_combination of an HTTP path + verb_

---

## `operationId`

Operation field to uniquely identify path/verb combinations

Note:
Makes generated clients WAY nicer.

Should be unique across all files referenced in a spec

---

## "schema"

_a format for data_

---

Primitives, Arrays, Objects oh my!

Base structure for many parts of a spec

---

Lifted directly from JSON Schema and extended many places in swagger specs:

- data types
- definitions
- parameters
- responses
- spec itself is a schema

Note:
Similar to a model

---

## "parameters"

_data used by an operation_

(not just query params)

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

* abstract definitions
* parameter definitions
* responses definitions
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

## editor.swagger.io

Online editor is SUPER useful

---

## Response descriptions are not optional

---

## Swagger specification can be extended

Many parts of the spec support vendor-specific extension and customization.

`x-` is the vendor-specific prefix.

---


## Authentication

Special headers. Defined in `SecurityDefinitions`, not parameters.

(caveats later...)

---

## Auth strategies

* oauth2
* security token
* user/pass

---

Require both a security definition **and** a security object.

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

* factors out common structures
* reuses definitions with `$ref`
* has good text descriptions

---

## So why use Swagger?

---

## Better documentation

`swagger-ui`

- consistent
- interactive
- you don't have to build it

---

Most developers first encounter swagger spec by interacting with a Swagger UI site.

---

## Way more than just docs

Other tooling and libraries available for:

* application routing
* parameter validation
* security + ACL middleware
* auto-generated API clients
* database model generation
* automated api testing
* type definitions

---v

Some libraries we use with swagger:
* `swagger-tools` - for internal API scaffolding
* `swagger-ui` - for api documentation
* `swagger-client` - for generated clients
* `read-yaml` - `.yaml` is nice for humans

---

## Why is it important?

---

## Garden of the commons

At its core, the Swagger spec carves out a common ground for both computers
and humans to understand REST APIs. With these common terms, we can write and
reuse tools and libraries across APIs in diverse languages.

When used to map routes to controllers and validate parameters, a swagger spec
can become *the* canonical representation of a REST API.

Note:
Just like a REST API establishes a contract for interacting with an application,
a Swagger spec establishes a contract for interacting with APIs in general.

Within this common ground, engineers can spend their time crafting APIs and tools
that work together, regardless of platform, language

---

## OpenAPI is the new Swagger

On January 1st, 2016 Swagger was been donated to the [Open API Initiative](https://openapis.org).

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

# *Act 3: Issues and Solutions*

How we've addressed them (or plan to...)

---

- learning curve + context
- author/consumer mismatch
- discovery, distribution
- securityDefinitions
- production
- tooling design, maturity, documentation

---

## Learning curve and context?

Easy to spread confusion and about an abstract idea.

Fear of the unknown is real.

---

## Talk about it

- explain Swagger
- differentiate specification from tooling
- share the problems it solves
- communicate clearly!

---v

## Work...

- share good spec examples
- working sessions
- just try it!

---v

## Talk some more...

- give internal tech talks
- slack channels
- let a team lead the charge

---v

## Roll it out

Docs are great, but routing, validation, ACL, and clients are the real value.

The more you invest, the better your return.

---

## Author/consumer mismatch?

Authors care about their app.

Consumers care about the docs and clients.

---

## Communication and experience

Get people working together to build common ground.

- highlight business-specific concerns
- acknowledge existing problems
- open process
- pair + code review
- make validation the bad guy, not people

---

## Discovery

A spec is useless if it can't be found.

Aggregate internal specs into a simple docs app.

Make it obvious. Make it current by default.

Note:
Express app + swagger UI is fine.

---v

In documention app `package.json`:

```
"swagger-specs": "*"
```

One of the right times to always match latest!

We autodeploy docs app with a github webhook on `swagger-specs`.

---

## Distribution

- collect specs in one repo
- add environment-specific hosts during build
- publish as module
- make machines do the work!

---

## SecurityDefinitions

Probably the thorniest part of the spec right now.

---v

Reusing a `swagger-clients` instance?

SecurityDefinitions seem like they can only be specified on per client, not per operation.

Define security schemes as securityDefinitions *AND* parameters.

Note:
Clients can be reused without fear of polluting user data across requests.

---

## Production

- `swagger-tools` mounts to `\`
- `swagger-client` is generated async
- code generation isn't terribly cheap
- avoid network hiccups when requesting a spec

---v

- send URLs to analytics in middleware
- wrap `swagger-client` to clean up interface
- generate clients once and reuse across requests
- consume specs as files, not urls
- version specs and client wrapper separately

Note:
App should be a deployable artifact!

---v

Here's that express middleware for New Relic agent:

```
if (req.swagger) {
  let controller = get(req, 'swagger.path[x-swagger-router-controller]');
  let operation = get(req, 'swagger.operation.operationId') ||
    get(req, 'swagger.operationPath[2]');
  newRelic.setControllerName(controller, operation);
}
```

(we should probably open a PR on `swagger-tools`)

---

## Tooling issues

Swagger isn't new, but the tooling isn't mature either.

- libraries in Javascript are new
- library *developers* are new to Javascript
- library docs are inconsistent (surprise!)

---

## Tooling solutions

- don't be afraid to read the source
- contribute code upstream
- write some docs!

---

# *Epilogue*

Where do we go from here?

---

## Contribute

- improving tooling
- break down existing libraries into modules
- author new stuff!

---

## `swagger-client`

The best client in Javascript we've found, but needs some love.

Primarily targeted for exploration of APIs in the browser.

---v

- multiple specs = multiple clients
- loads async
- lacks Node.js callback interface
- bloated
- only supports jQuery + `superagent`
- obscures access to HTTP agent

---

## Spec generation

Generation of a spec from dynamic languages is a hard problem.

Introspection with AST could help.

---

## Spec validation

- `swagger-parser`
- other JSON Schema tooling

Not great yet.

---

## Spec cleanup

Automatically factor out common structures into optimized definitions.

---

## Spec discovery

App to serve specs and documentation for internal and public specs.

---

## Documentation

(surprise!)

Note:
Help out! People are happy to let you do their work for them.

---

Thanks to:

- NodeSource for the opportunity
- @dshaw for the gut-check
- Shutterstock for the time + support
- Oz Ruiz for suggesting the talk
- JS Platform team for covering for me
- Fotios Lindiakos for the LaTeX

---

And thanks to you for paying attention.

![](img/shutterstock.jpg)

James Vivian

- @jvivs github/twitter
- jvivian@shutterstock.com
