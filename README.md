<div align="center">
	
<div>  
  &nbsp; 
</div>
	
<a href="https://jsight.io" align="left"><img src="./img/jsight-logo.svg" alt="Try now!" width="148px"/></a>
	
# JSight Language Specifications

  [![Telegram support](https://img.shields.io/badge/Support-Telegram-blue)](https://t.me/jsight_support)
  [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](./CONTRIBUTING.md)
  [![License](https://img.shields.io/github/license/jsightapi/specification?colorB=ff0000)](./LICENSE)
  [![JSight on Facebook](https://img.shields.io/badge/Facebook-1877F2?logo=facebook&logoColor=white)](https://www.facebook.com/jsightapi)
  [![JSight on LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?logo=linkedin&logoColor=white)](https://www.linkedin.com/company/jsightapi/)
  [![Twitter Follow](https://img.shields.io/twitter/follow/jsightapi.svg?style=social)](https://twitter.com/jsightapi)

<div>  
  &nbsp; 
</div>
	
  <a href="https://www.alchemistaccelerator.com/portfolio?class=29"><img width="300px" style="background-color: white;" src="./img/alchemist.svg" alt="Alchemist Accelerator"/></a>
<div>  
  &nbsp; 
</div>
	
  :star: **Star us on GitHub — it motivates us a lot!** :star:
	
<div>  
  &nbsp; 
</div>
</div>

**JSight API** and **JSight Schema** languages — you have never specified APIs and data schemas so
fast! We mean it.

1. 2–4 times less code.
2. APIs and data schemas designing is 2–3 times faster.
3. The describing process does not detract from the design.
4. It is incredibly convenient to jointly design an API with the whole team in one go. No more
   multiple design iterations and edits required. All you need is to share the screen between the
   participants and come up with an API together right in the [JSight Online
   Editor](https://editor.jsight.io).
5. All this is possible thanks to the compactness and simplicity of the JSight API and JSight Schema
   languages.  
   [Compare **JSight API** with **Open API**](#scroll--jsight-api-language).  
   [Compare **JSight Schema** with **JSON Schema**](#scroll--jsight-schema-language).

Supported standards: [HTTP REST](#scroll--jsight-api-language), [JSON-RPC
2.0](#json-rpc-20-new-feature).

<div>  
  &nbsp; 
</div>

<blockquote>
  &nbsp;
	
  *“JSight is as much simpler than OpenAPI as Markdown is simpler than HTML. At the same time,
  JSight is just as powerful.”*  

  <img src="https://avatars.githubusercontent.com/u/101567029?v=4" align="left" width="40"
  height="40" alt=""/>

  Constantine M., JSight CRDO.

  &nbsp;

</blockquote>  

**JSight API** and **JSight Schema** are two independent languages, while the **JSight Schema**
language is a part of the **JSight API** language.
<div>  
  &nbsp; 
</div>
<div align="center">
  <img src="./img/specification-schema.svg" />
</div>
<div>  
  &nbsp; 
</div>

**JSight Schema** language is designed to describe data schemas, regardless of the serialization
format.

**JSight API** language is designed to describe APIs of different standards. Currently, HTTP REST
API is supported. Other types of APIs will be supported in the future. The JSight API language uses
the JSight Schema language to describe input and output data.

You can always try the JSight API language (and JSight Schema) in the JSight Online Editor:
https://editor.jsight.io.

<div>  
  &nbsp; 
</div>

## :hatching_chick: &nbsp; Quick Start

&nbsp;

<div align="center">

  <a href="https://jsight.io/docs/jsight-api-0-3-quick-tutorial"><img src="./img/tab1.svg" alt="Quick tutorial"/></a>&nbsp;&nbsp;

  <div>
    &nbsp;
  </div>

  <a href="https://editor.jsight.io"><img src="./img/try-wide.svg" alt="Try now!" width="400px"/></a>

&nbsp;

![Demo](./img/demo.gif)

</div>

<div>  
  &nbsp; 
</div>

## :book: &nbsp; Table of Contents

* [JSight API Language](#scroll--jsight-api-language)
* [JSight Schema Language](#scroll--jsight-schema-language)
* [Roadmap](#date--roadmap)
* [Versioning](#bookmark_tabs--versioning)
* [Dependencies](#notebook_with_decorative_cover--dependencies)
* [Contributing](#sunglasses--contributing)
* [Bugs and Feature Requests](#speech_balloon--bugs-and-feature-requests)
* [Support](#grey_question--support)
* [License](#receipt--license)
* [Resources](#book--resources)
* [Partners](#handshake--partners)
* [Acknowledgments](#trophy--acknowledgments)

<div>  
  &nbsp; 
</div>

## :scroll: &nbsp; JSight API language

The JSight API language allows you to specify REST and JSON-RPC APIs with incredible speed and
convenience. More information can be found in the [Quick
Tutorial](https://jsight.io/docs/jsight-api-0-3-quick-tutorial) or in the [language
specification](./versions/JSight%20API/). Here we give examples of the same
API described using JSight API and Open API.

<details open><summary>Example 1. The simplest</summary>

<table align="center">
<thead>
<tr>
  <th width="50%">
    JSight API 0.3
  </th>
  <th>
    OpenAPI 3.0.1 (Swagger)
  </th>
</tr>
</thead>
<tbody>
<tr valign="top">
<td>

```
JSIGHT 0.3

GET /cats/{id}
  200
    {
      "id"  : 123, // {min: 1}
      "name": "Tom"
    }
```

Pay attention to the main feature of the JSight API language. **The basis for a data schema is an
example of valid data.** Additional data requirements are specified in C-like comments. This
approach greatly simplifies the data schema and makes it intuitively clear. Practice shows that such
schema is very simple to create, read and edit.

Learn more about the JSight language: [Quick
Tutorial](https://jsight.io/docs/jsight-api-0-3-quick-tutorial/).

<div align="center">

:star: **Star us on GitHub — it motivates us a lot!**

</div>

</td>
<td>

```
openapi: 3.0.1
info:
  title: ""
  version: ""
paths:
  /cats/{id}:
    get:
      parameters:
      - name: id
        in: path
        required: true
        schema: {}
      responses:
        200:
          description: ""
          content:
            application/json:
              schema:
                type: object
                required: [id, name]
                properties:
                  id:
                    type: integer
                    minimum: 1
                    example: 123
                  name:
                    type: string
                    example: "Tom"
```
	
</td>
</tr>
</tbody>
</table>

</details>

<details><summary>Example 2: User Types</summary>

<table align="center">
<thead>
<tr>
  <th width="50%">
    JSight API 0.3
  </th>
  <th>
    OpenAPI 3.0.1 (Swagger)
  </th>
</tr>
</thead>
<tbody>
<tr valign="top">
<td>

```
JSIGHT 0.3

GET /cats      // Get all cats.
  200
    [@cat]

GET /cats/{id} // Get a cat by its id.
  200
    @cat
  
TYPE @cat      // Type “Cat”.
  {
    "id"  : 123,  // ID of the cat.
    "name": "Tom" // Name of the cat.
  }
```

Pay attention to how convenient it is to work with user types in JSight API. The type name is simply
inserted where the type should be in the data schema. Everything is the same as in conventional
programming languages.

More about user types: [Quick Tutorial. Lesson 2. User
types](https://jsight.io/docs/jsight-api-0-3-quick-tutorial/lesson02).

<div align="center">

:star: **Star us on GitHub — it motivates us a lot!**

</div>

</td>
<td>

```
openapi: 3.0.3
info:
  title: ""
  version: ""
paths:
  /cats:
    get:
      summary: Get all cats.
      responses:
        200:
          description: ""
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Cat'
  /cats/{id}:
    get:
      summary: Get a cat by its id.
      parameters:
      - name: id
        in: path
        required: true
        schema: {}
      responses:
        200:
          description: ""
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Cat'
components:
  schemas:
    Cat:
      description: Type “Cat”.
      type: object
      required: [id, name]
      properties:
        id:
          description: ID of the cat.
          type: integer
          minimum: 0
          example: 123
        name:
          description: Name of the cat.
          type: string
          example: "Tom"
```
	
</td>
</tr>
</tbody>
</table>

</details>

<details><summary>Example 3: Schema</summary>

<table align="center">
<thead>
<tr>
  <th width="50%">
    JSight API 0.3
  </th>
  <th>
    OpenAPI 3.0.1 (Swagger)
  </th>
</tr>
</thead>
<tbody>
<tr valign="top">
<td>

```
JSIGHT 0.3

TYPE @cat
{
  "id"      : 123,
  "name"    : "Tom",
  "birthday": "2006-01-02",          // {type: "date" }
  "email"   : "tom@cats.com",        // {type: "email"}
  "website" : "http://tom.cats.com", // {type: "uri"  }
  "salary"  : 13.23,                 // {precision: 2 }
  "friends" : [                      // {maxItems: 10 }
    @cat
  ],
  "bestFriend": @cat,       // {optional: true}
  "size"    : "XL"          // {enum: ["M", "L", "XL"]}
}
```

Details that are not obvious from the example of valid data are provided in small JSON objects in
C-like comments. This approach allows you to write data schemas of any complexity, while keeping
them compact and intuitive.

More about JSight Schema: [Quick Tutorial. Lesson 4.
Schemas](https://jsight.io/docs/jsight-api-0-3-quick-tutorial/lesson04).

<div align="center">

:star: **Star us on GitHub — it motivates us a lot!**

</div>

</td>
<td>

```
openapi: 3.0.3
info:
  title: ""
  version: ""
paths: {}
components:
  schemas:
    Cat:
      type: object
      required:
        - id
        - name
        - birthday
        - email
        - website
        - salary
        - friends
        - size
      properties:
        id:
          type: integer
          minimum: 0
          example: 123
        name:
          type: string
          example: "Tom"
        birthday:
          type: string
          format: date
          example: 2006-01-02
        email:
          type: string
          format: email
          example: "tom@cats.com"
        website:
          type: string
          format: uri
          example: "http://tom.cats.com"
        salary:
          type: number
          multipleOf: 0.01
          example: 13.23
        friends:
          type: array
          items:
            $ref: '#/components/schemas/Cat'
        bestFriend:
          $ref: '#/components/schemas/Cat'
        size:
          type: string
          enum: [M, L, XL]
          example: XL
```
	
</td>
</tr>
</tbody>
</table>

</details>

<details><summary>Example 4. POST</summary>

<table align="center">
<thead>
<tr>
  <th width="50%">
    JSight API 0.3
  </th>
  <th>
    OpenAPI 3.0.1 (Swagger)
  </th>
</tr>
</thead>
<tbody>
<tr valign="top">
<td>

```
JSIGHT 0.3

POST /cats // Create a new cat.
  Request
    {
      "id"  : 123,
      "name": "Tom"
    }

  200
    "OK" // {const: true}

  404 any
  500 empty
```

Please note that the POST request and the three response options are written in a clear and concise
manner.

More about requests and responses: [Quick Tutorial. Lesson 6. Requests and
Responses](https://jsight.io/docs/jsight-api-0-3-quick-tutorial/lesson06).

<div align="center">

:star: **Star us on GitHub — it motivates us a lot!**

</div>

</td>
<td>

```
openapi: 3.0.3
info:
  title: ""
  version: ""
paths:
  /cats:
    post:
      summary: "Create a new cat"
      requestBody:
        content:
          application/json:
            schema:
              type: object
              required: [id, name]
              properties:
                id:
                  type: integer
                  minimum: 0
                  example: 123
                name:
                  type: string
                  example: "Tom"          
      responses:
        200:
          description: ""
          content:
            application/json:
              schema:
                type: string
                enum: ["OK"]
        404:
          description: ""
          content:
            application/json:
              schema: {}
        500:
          description: ""
```
	
</td>
</tr>
</tbody>
</table>

</details>

<details><summary>Example 5: Inheritance</summary>

<table align="center">
<thead>
<tr>
  <th width="50%">
    JSight API 0.3
  </th>
  <th>
    OpenAPI 3.0.1 (Swagger)
  </th>
</tr>
</thead>
<tbody>
<tr valign="top">
<td>

```
JSIGHT 0.3

TYPE @pet
{
  "id"  : 123,
  "name": "Tom"
}

TYPE @cat
{ // {allOf: "@pet"}
  "likesMouses": true
}

TYPE @dog
{ // {allOf: "@pet"}
  "teethSize": "big" // {enum: ["big", "small"]}
}
```

This example shows how simple it is to inherit one type from another in JSight using the rule
`allOf`.

Learn more: [JSight Schema Specification. Rule
"allOf"](./versions/JSight%20Schema/JSight%20Schema%200.3.5.md#rule-allof).

<div align="center">

:star: **Star us on GitHub — it motivates us a lot!**

</div>

</td>
<td>

```
openapi: 3.0.3
info:
  title: ""
  version: ""
paths: {}
components:
  schemas:
    Pet:
      type: object
      required: [id, name]
      properties:
        id:
          type: integer
          example: 123
        name:
          type: string
          example: "Tom"
    Cat:
      allOf:
        - $ref: '#/components/schemas/Pet'
        - type: object
          required: [likesMice]
          properties:
            likesMice:
              type: boolean
    Dog:
      allOf:
        - $ref: '#/components/schemas/Pet'
        - type: object
          required: [teethSize]
          properties:
            teethSize:
              type: string
              enum: ["big", "small"]
```
	
</td>
</tr>
</tbody>
</table>

</details>

<details><summary>Example 6. Full-fledged CRUD API</summary>

<table align="center">
<thead>
<tr>
  <th width="50%">
    JSight API 0.3
  </th>
  <th>
    OpenAPI 3.0.1 (Swagger)
  </th>
</tr>
</thead>
<tbody>
<tr valign="top">
<td>

```
JSIGHT 0.3

GET /cats         // Get all cats.
  200 [@cat]      // Returns all cats.

POST /cats        // Create a cat.
  Request @cat
  200 @cat        // Success.

GET /cats/{id}    // Get a cat by its id.
  200 @cat        // Returns a cat.

PUT /cats/{id}    // Update a cat.
  Request @cat
  200 @cat        // Returns an updated cat.

DELETE /cats/{id} // Delete a cat.
  200 any

TYPE @cat // A cat.
{
  "id"   : 1,
  "name" : "Tom",
  "color": "black" // {enum: ["black", "white"]}
}
```

A full-fledged CRUD API took only 25 lines.

<div align="center">

:star: **Star us on GitHub — it motivates us a lot!**

</div>

</td>
<td>

```
openapi: 3.0.3
info:
  title: ""
  version: ""
paths:
  /cats:
    get:
      summary: Get all cats.
      responses:
        200:
          description: ""
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Cat'
    post:
      summary: Create a cat.
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Cat'
      responses:
        200:
          description: ""
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Cat'
  /cats/{id}:
    parameters:
    - name: id
      in: path
      required: true
      schema: {}
    get:
      summary: Get a cat by its id.
      responses:
        200:
          description: ""
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Cat'
    put:
      summary: Update a cat.
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Cat'
      responses:
        200:
          description: ""
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Cat'
    delete:
      summary: Delete a cat.
      responses:
        200:
          description: ""
          content:
            application/json:
              schema: {}
components:
  schemas:
    Cat:
      description: Type “Cat”.
      type: object
      required: [id, name, color]
      properties:
        id:
          type: integer
          example: 123
        name:
          type: string
          example: "Tom"
        color:
          type: string
          enum: ["black", "white"]
```
	
</td>
</tr>
</tbody>
</table>

</details>

<details><summary>Example 7. Macros</summary>

<table align="center">
<thead>
<tr>
  <th width="50%">
    JSight API 0.3
  </th>
  <th>
    OpenAPI 3.0.1 (Swagger)
  </th>
</tr>
</thead>
<tbody>
<tr valign="top">
<td>

```
JSIGHT 0.3

GET /cats // Get all cats.
  200 [@cat]
  PASTE @errorResponses

GET /cats/{id} // Get a cat by its id.
  200 @cat
  PASTE @errorResponses

TYPE @cat // Type “Cat”.
{
  "id"  : 1,
  "name": "Tom"
}

MACRO @errorResponses
  400 any
  401 any
  405 any
  500 any
```

Macros are a powerful feature of the JSight API language. It allows you to reuse parts of code as
many times as you like.

More about macros: [Quick Tutorial. Magic directive
MACRO](https://jsight.io/docs/jsight-api-0-3-quick-tutorial/lesson09).

<div align="center">

:star: **Star us on GitHub — it motivates us a lot!**

</div>

</td>
<td>

```
openapi: 3.0.1
info:
  title: ""
  version: ""
paths:
  /cats:
    get:
      summary: Get all cats.
      responses:
        200:
          description: ""
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Cat'
        400:
          $ref: '#/components/responses/Error400'
        401:
          $ref: '#/components/responses/Error401'
        405:
          $ref: '#/components/responses/Error405'
        500:
          $ref: '#/components/responses/Error500'
  /cats/{id}:
    get:
      description: "Get a cat by its id."
      parameters:
      - name: id
        in: path
        required: true
        schema: {}
      responses:
        200:
          description: ""
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Cat'
        400:
          $ref: '#/components/responses/Error400'
        401:
          $ref: '#/components/responses/Error401'
        405:
          $ref: '#/components/responses/Error405'
        500:
          $ref: '#/components/responses/Error500'
components:
  schemas:
    Cat:
      description: Type “Cat”.
      type: object
      required: [id, name]
      properties:
        id:
          type: integer
          example: 1
        name:
          type: string
          example: "Tom"
  responses:
    Error400:
      description: ""
      content:
        application/json:
          schema: {}
    Error401:
      description: ""
      content:
        application/json:
          schema: {}
    Error405:
      description: ""
      content:
        application/json:
          schema: {}
    Error500:
      description: ""
      content:
        application/json:
          schema: {}
```
	
</td>
</tr>
</tbody>
</table>

</details>

<details><summary>Example 8: Large REST API</summary>

<table align="center">
<thead>
<tr>
  <th width="50%">
    JSight API 0.3
  </th>
  <th>
    OpenAPI 3.0.1 (Swagger)
  </th>
</tr>
</thead>
<tbody>
<tr valign="top">
<td>

```
###
JSight 0.3 Demo API.
For more information see official docs: 
https://jsight.io/docs/jsight-api-0-3.
###

JSIGHT 0.3

INFO
  Title "Pets REST API"
  Version 1.0
  Description
    ## Overview

    The **Pets REST API** allows you to manage your pets.

    Powered by [JSight](http://jsight.io)©.

SERVER @prod // Production server.
  BaseUrl "https://pets.com/api/1.0"

SERVER @test // Test server.
  BaseUrl "https://192.168.0.100/1.0"

#======================= CATS =======================

#---------------------- /cats -----------------------

GET /cats // Get all cats.
  200     // Returns all cats.
    {
      "items": [@cat],
      "itemsCount": 25 // {min: 0}
    }

POST /cats      // Create a cat.
  Request @cat
  200 @cat      // Success.
  409 @error    // Error.

#------------------- /cats/{id} --------------------
URL /cats/{id}
  Path
    {
      "id": "CAT-123" // {type: "@petId"} - Cat's id.
    }

GET /cats/{id} // Get a cat by its id.
  200 @cat     // Return a cat.
  404 empty    // A cat is not found.

PUT /cats/{id} // Update a cat.
  Request @cat
  200 @cat     // Returns an updated cat.
  404 empty    // A cat is not found.
  409 @error   // Some error.

PATCH /cats/{id} // Update a cat's status.
  Request
    {
      "status": "relaxing" // New status of the cat.
    }
  200 any    // Ok.
  409 @error // Some error.
  404 empty  // A cat is not found.

DELETE /cats/{id} // Delete a cat.

#----------------- /cats/{id}/friends/{friendId} ---------

GET /cats/{id}/friends/{friendId} // Get cats friend.
  Path
  {
    "friendId": @petId // Friend's id.
  }

  200 // Return the cats friend.
    @cat | @pig // The cat's friend (cat or pig).

#======================= /dogs =======================

GET /dogs // Get the paged list of all dogs.
  Query "page=1&pageSize=30&filter[age]=12"
    {                // {allOf: "@pageQuery"}
      "filter": {    // {optional: true}
        "size": "S", /* {optional: true, enum: ["S", "L", "M"]} 
                        - Filter by dog's size. */
        "age" : 12   /* {optional: true, min: 0               } 
                        - Filter by dog's age. */
      }
    }
  Request
    Headers
      @commonRequestHeaders
    Body empty
  200 // Returns a page of dogs.
    Headers
      @commonResponseHeaders
    Body
      {
        "items": [@dog],
        "page" : 1,      
        "pageSize": 30   
      }
  PASTE @errorResponses

POST /dogs // Create a new dog.
  Description
  (
    ### Limitations
    
    - Accounts are limited to 25 new dogs 
      within a 24 hour period.
  )
  Request
    Headers
      { // {allOf: ["@commonRequestHeaders"]}
        "X-header": "Some additional header"
      }
    Body
      @dog
  200 // Success.
    Headers
      {
        "X-header": "Some additional header"
      }
    Body regex
      /^OK$/
  PASTE @errorResponses

#------------------- /dogs/{id} ---------------------
URL /dogs/{id}
  Path
    {
      "id": "DOG-123" // {type: "@petId"} - Dog's id.
    }

GET /dogs/{id} // Get a dog by its id.
  Request
    Headers
      @commonRequestHeaders
    Body empty
  200          // Return a dog.
    Headers
      @commonResponseHeaders
    Body
      @dog     
  PASTE @errorResponses

PUT /dogs/{id} // Update a dog.
  Request
    Headers
      @commonRequestHeaders
    Body 
      @dog
  200          // Success.
    Headers
      @commonResponseHeaders
    Body any
  PASTE @errorResponses

DELETE /dogs/{id} // Delete a dog.
  Request
    Headers
      @commonRequestHeaders
    Body empty
  200          // OK.
    Headers
      @commonResponseHeaders
    Body
      "OK" // {const: true}
  PASTE @errorResponses

#====================== PIGS ========================

URL /pigs
  GET              // Get the list of all pigs.
    200 [@pig]     // List of pigs.
  POST             // Create a new pig.
    Request @pig
    200 @pig       // Created pig.
    409 @error     // Pig is not created.

URL /pigs/{id}
  Path
    {
      "id": "PIG-123" // {type: "@petId"} - A pigs id.
    }
  GET                 // Get a pig by its id.
    200 @pig          // A pig.
    404 @error        // Pig not found.
  PUT                 // Change a pig by its id.
    Request @pig      
    200 @pig          // An updated pig.
    404 @error        // Pig not found.
    409 @error        // Error.
  DELETE              // Delete a pig by its id.
    200 empty         // Success.
    404 @error        // Pig not found.
    409 @error        // Error.

#====================== TYPES ========================

TYPE @pet // A pet.
{
  "id"        : @petId,
  "name"      : "Tom",
  "type"      : "PIG", // {enum: ["CAT", "DOG", "PIG"]}
  "age"       : 10,    // {min: 0, max: 99}
  "email"     : "tom@pets.com",        // {type: "email"}
  "uri"       : "http://tom.pets.com", // {type: "uri"}
  "birthday"  : "2012-01-03",          // {type: "date"}
  "uuid"      : "550e8400-e29b-41d4-a716-446655440000" // {type: "uuid"}
}

TYPE @cat // A cat.
{ // {allOf: "@pet"}
  "status": "relaxing",
  "bestFriend": @cat,
  "topFriends": { // {additionalProperties: true}
    @petName: @cat | @pig
  },
  "topEnemies": [ // {maxItems: 10}
    @dog
  ]
}

TYPE @dog // A dog.
{ // {allOf: "@pet"}
  "friendIds": [ // Only dog ids are allowed.
    @petId
  ],
  "isDangerous": false,
  "legacyId": 1, /* {or: [
                        {type: "integer", min: 0, exclusiveMinimum: true}, 
                        {type: "string"}
                      ], 
                      optional: true
                    } */
  "additionalData": {} // {type: "any"} - Field for legacy.
}

TYPE @pig // A pig.
{ // {allOf: "@pet"}
  "temperature" : 35.6, // {precision: 1, nullable: true}
  "pigSize"     : "S",  // {type: "@pigSize"}
  "lastWashTime": "2021-01-02T15:04:05+03:00", // {type: "datetime"}
  "additionalData": {  // {additionalProperties: "string"}
    "key": "value"
  }
}

TYPE @pigSize
  "S" /* {enum:[
            "XXS",
            "XS",
            "S",
            "M",
            "L",
            "XL",
            "XXL"
          ]} */

TYPE @petId 
  "GOAT-12345" // {regex: "^[A-Z]+-\\d+$", minLength: 3, maxLength: 255}

TYPE @petName regex
  /^[A-Z][a-z]*( [A-Z][a-z]*)*$/

#-------------------- COMMON TYPES -------------------

TYPE @error // A common error response.
{
  "code": 12,
  "message": "Something bad had happened on server..."
}

TYPE @commonRequestHeaders
{ // {allOf: ["@contentTypeHeader", "@authHeader"]}
}

TYPE @commonResponseHeaders
{ // {allOf: ["@contentTypeHeader"]}
}

TYPE @contentTypeHeader
{
  "Content-Type": "application/json" // {const: true}
}

TYPE @authHeader // Authorization header.
{
  "Authorization": "Basic dG9tQGNhdC5jb206YWJjMTIz=" /* 
                        {regex: "^Basic [A-Za-z0-9+\\/=]+$"} */
}

TYPE @pageQuery
{
  "page"    : 1, // {optional: true, min: 1} - 1 by default.
  "pageSize": 30 // {optional: true, min: 10, max: 100} - 30 by default.
}

#----------------------- MACROS -----------------------------------

MACRO @errorResponses
(
  401 any      // Unauthorised.
  404 empty    // Not found.
  409 @error   // Some error.
)
```

</td>
<td>

We did not describe this API in OpenAPI. It is too complicated and very long…

<div align="center">

:star: **Star us on GitHub — it motivates us a lot!**

</div>
	
</td>
</tr>
</tbody>
</table>

</details>

#### JSON-RPC 2.0. New Feature!

<details><summary>Example 9. JSON-RPC 2.0</summary>

<table align="center">
<thead>
<tr>
  <th width="50%">
    JSight API 0.3
  </th>
  <th>
    OpenRPC 1.2.1
  </th>
</tr>
</thead>
<tbody>
<tr valign="top">
<td>

```
JSIGHT 0.3
URL / 
  Protocol json-rpc-2.0
  Method listPets // List all pets
    Params
      [
        20 // Limit (how many items to return).
      ]
    Result
      [       // An array of pets
        {     // Pet
          "id": 123,
          "name": "Tom"
        }
      ]
 
```

The JSON-RPC API is as simple to describe as the REST API.

More about JSON-RPC 2.0 support: [Quick Tutorial. JSON-RPC 2.0
support](https://jsight.io/docs/jsight-api-0-3-quick-tutorial/lesson10).

<div align="center">

:star: **Star us on GitHub — it motivates us a lot!**

</div>

</td>
<td>

```
{
  "openrpc": "1.2.1",
  "info": {
    "version": "",
    "title": ""
  },
  "methods": [
    {
      "name": "listPets",
      "description": "List all pets",
      "params": [
        {
          "name": "limit",
          "description": "How many items to return",
          "schema": {
            "type": "integer"
          }
        }
      ],
      "result": {
        "name": "pets",
        "description": "An array of pets",
        "schema": {
          "type": "array",
          "items": {
            "title": "Pet",
            "type": "object",
            "properties": {
              "id": {
                "type": "integer"
              },
              "name": {
                "type": "string"
              }
            }
          }
        }
      },
      "examples": [
        {
          "name": "listPetExample",
          "description": "List pet example",
          "params": [
            {
              "name": "limit",
              "value": 20
            }
          ],
          "result": {
            "name": "listPetResultExample",
            "value": [
              {
                "id": 123,
                "name": "Tom"
              }
            ]
          }
        }
      ]
    }
  ]
}
```

</td>
</tr>
</tbody>
</table>

</details>

<div>  
  &nbsp; 
</div>

## :scroll: &nbsp; JSight Schema language

The JSight Schema language allows you to describe any data structures with incredible speed and
convenience. You can read more about it in the [JSight Schema Language
Specification](./versions/JSight%20Schema/).

The JSight Schema language is actively used by the [JSight
API](#scroll--jsight-api-language) language, which is designed to describe API.
For more information about JSight Schema within the JSight API, see the [Quick
Tutorial](https://jsight.io/docs/jsight-api-0-3-quick-tutorial/lesson04).

Mentioned below are examples of the same data schemas described using **JSight Schema** and **JSON
Schema**.

<details open><summary>Example 1. The simplest</summary>

<table align="center">
<thead>
<tr>
  <th width="50%">
    JSight Schema 0.3
  </th>
  <th>
    JSON Schema 2020-12
  </th>
</tr>
</thead>
<tbody>
<tr valign="top">
<td>

```
{
  "id"  : 123, // {min: 1}
  "name": "Tom"
}
```

Pay attention to the main feature of the JSight Schema language. **The basis for a data schema is an
example of valid data.** Additional data requirements are specified in C-like comments. This
approach greatly simplifies the data schema and makes it intuitively clear. Practice shows that such
schema is very simple to create, read and edit.

For details, see the JSight Schema Language Specification, in the [section
“EXAMPLE”](./versions/JSight%20Schema/JSight%20Schema%200.3.5.md#example).

<div align="center">

:star: **Star us on GitHub — it motivates us a lot!**

</div>

</td>
<td>

```
{
    "type": "object",
    "required": [
        "id",
        "name"
    ],
    "properties": {
        "id": {
            "type": "integer",
            "minimum": 1
        },
        "name": {
            "type": "string"
        }
    },
    "examples": [{
        "id": 123,
        "name": "Tom"
    }]
}
```
	
</td>
</tr>
</tbody>
</table>

</details>

<details><summary>Example 2. Nested arrays and objects</summary>

<table align="center">
<thead>
<tr>
  <th width="50%">
    JSight Schema 0.3
  </th>
  <th>
    JSON Schema 2020-12
  </th>
</tr>
</thead>
<tbody>
<tr valign="top">
<td>

```
{
  "productId": 1,
  "productName": "An ice sculpture",
  "price": 12.50,    // {min: 0, exclusiveMinimum: true}
  "tags": [          // {minItems: 1, optional: true}
    "cold", 
    "ice"
  ],
  "dimensions": {    // {optional: true}
    "length": 7.0,
    "width": 12.0,
    "height": 9.5
  }
}
```

The JSight Schema language is especially useful when describing nested objects and arrays, which are
very common in real life. You simply give an example of a valid array or object, and add small
clarifying comments.

It is much more complicated in JSON Schema and other languages.

For details, see the JSight Schema Language Specification, sections:

- [EXAMPLE requirements for object
  PROPERTIES](./versions/JSight%20Schema/JSight%20Schema%200.3.5.md#example-requirements-for-object-properties),
- [EXAMPLE requirements for array
  ELEMENTS](./versions/JSight%20Schema/JSight%20Schema%200.3.5.md#example-requirements-for-array-elements).

<div align="center">

:star: **Star us on GitHub — it motivates us a lot!**

</div>

</td>
<td>

```
{
  "type": "object",
  "properties": {
    "productId": {
      "type": "integer"
    },
    "productName": {
      "type": "string"
    },
    "price": {
      "type": "number",
      "exclusiveMinimum": 0
    },
    "tags": {
      "type": "array",
      "items": {
        "type": "string"
      },
      "minItems": 1
    },
    "dimensions": {
      "type": "object",
      "properties": {
        "length": {
          "type": "number"
        },
        "width": {
          "type": "number"
        },
        "height": {
          "type": "number"
        }
      },
      "required": [ "length", "width", "height" ]
    }
  },
  "required": ["productId","productName","price"],
  "examples": [{
    "productId": 1,
    "productName": "An ice sculpture",
    "price": 12.50,
    "tags": [ "cold", "ice" ],
    "dimensions": {
      "length": 7.0,
      "width": 12.0,
      "height": 9.5
    }
  }]
}
```

This example was created based on the official [Getting Started
instructions](https://json-schema.org/learn/getting-started-step-by-step.html) of the JSON-Schema
standard.

</td>
</tr>
</tbody>
</table>

</details>

<details><summary>Example 3. Property description</summary>

<table align="center">
<thead>
<tr>
  <th width="50%">
    JSight Schema 0.3
  </th>
  <th>
    JSON Schema 2020-12
  </th>
</tr>
</thead>
<tbody>
<tr valign="top">
<td>

```
{ // A product from Acme's catalog.
  "productId": 1,                    // The unique id.
  "productName": "An ice sculpture", // Name of the product.
  
  "price": 12.50, /* {
                       min: 0, 
                       exclusiveMinimum: true
                     } 
                     - The price. */

  "tags": [   /* {minItems: 1, optional: true}    
                 - Tags for the product. */
    "cold", 
    "ice"
  ],
  "dimensions": { // {optional: true} - The dimensions.
    "length": 7.0,
    "width": 12.0,
    "height": 9.5
  }
}
```

Here is the same schema as in the previous example, only property descriptions have been added.

Property descriptions are written in C-like comments. If there are *rules* in the comments, then the
property descriptions are separated by a hyphen.

For details, see the JSight Schema Language Specification, [section “Text notes to
RULES”](./versions/JSight%20Schema/JSight%20Schema%200.3.5.md#text-notes-to-rules).

<div align="center">

:star: **Star us on GitHub — it motivates us a lot!**

</div>

</td>
<td>

```
{
  "type": "object",
  "description": "A product from Acme's catalog.",
  "properties": {
    "productId": {
      "description": "The unique id.",
      "type": "integer"
    },
    "productName": {
      "description": "Name of the product.",
      "type": "string"
    },
    "price": {
      "description": "The price.",
      "type": "number",
      "exclusiveMinimum": 0
    },
    "tags": {
      "description": "Tags for the product.",
      "type": "array",
      "items": {
        "type": "string"
      },
      "minItems": 1
    },
    "dimensions": {
      "description": "The dimensions.",
      "type": "object",
      "properties": {
        "length": {
          "type": "number"
        },
        "width": {
          "type": "number"
        },
        "height": {
          "type": "number"
        }
      },
      "required": ["length", "width", "height"]
    }
  },
  "required": ["productId","productName","price"],
  "examples": [{
    "productId": 1,
    "productName": "An ice sculpture",
    "price": 12.50,
    "tags": [ "cold", "ice" ],
    "dimensions": {
      "length": 7.0,
      "width": 12.0,
      "height": 9.5
    }
  }]
}
```
	
</td>
</tr>
</tbody>
</table>

</details>

<details><summary>Example 4. Built-in data types</summary>

<table align="center">
<thead>
<tr>
  <th width="50%">
    JSight Schema 0.3
  </th>
  <th>
    JSON Schema 2020-12
  </th>
</tr>
</thead>
<tbody>
<tr valign="top">
<td>

```
{
  "id"        : 123,
  "name"      : "Tom",
  "email"     : "tom@cats.com",              // {type: "email"   }
  "website"   : "http://tom.cats.com",       // {type: "uri"     }
  "birthday"  : "2006-01-02",                // {type: "date"    }
  "registered": "2021-01-02T07:23:12+03:00", // {type: "datetime"}
  "uuid": "550e8400-e29b-41d4-a716-446655440000" // {type: "uuid"}
}
```

JSight Schema supports several additional built-in data types that are required for actual work.

Read more about built-in data types in the JSight Schema Language Specification, section [“RULE
"type"”](./versions/JSight%20Schema/JSight%20Schema%200.3.5.md#rule-type).

<div align="center">

:star: **Star us on GitHub — it motivates us a lot!**

</div>

</td>
<td>

```
{
  "type": "object",
  "properties": {
    "id": {
      "type": "integer",
      "examples": [123]
    },
    "name": {
      "type": "string",
      "examples": ["Tom"]
    },
    "email": {
      "type": "string",
      "format": "email",
      "examples": ["tom@cats.com"]
    },
    "website": {
      "type": "string",
      "format": "uri",
      "examples": ["http://tom.cats.com"]
    },
    "birthday": {
      "type": "string",
      "format": "date",
      "examples": ["2006-01-02"]
    },
    "registered": {
      "type": "string",
      "format": "date-time",
      "examples": ["2021-01-02T07:23:12+03:00"]
    },
    "uuid": {
      "type": "string",
      "format": "uuid",
      "examples": [
        "550e8400-e29b-41d4-a716-446655440000"
      ]
    }
  },
  "required": [
    "id",
    "name",
    "email",
    "website",
    "birthday",
    "registered",
    "uuid"
  ]
}
```
	
</td>
</tr>
</tbody>
</table>

</details>

<div>
  &nbsp;
</div>

## :date: &nbsp; Roadmap

- Support for other types of API: gRPC, Kafka, RabbitMQ, WebSocket.

If you have ideas and suggestions, please write to us:

- Email: [support@jsight.io](mailto:support@jsight.io)
- Telegram: [@jsight_support](https://t.me/jsight_support)

<div>  
  &nbsp; 
</div>

## :bookmark_tabs: &nbsp; Versioning

JSight API language and JSight Schema language releases are versioned according to the [Semantic
Versioning 2.0.0](https://semver.org/) standard:

```
{MAJOR version}.{MINOR version}.{PATCH version}
```

The releases are located in the branch `main` in the [`./versions/JSight
Schema/`](./versions/JSight%20Schema/) and [`./versions/JSight API/`](./versions/JSight%20API/),
respectively. Each new specification release is published into a separate file with a version
number, for example `./versions/JSight Schema/0.3.0.md` or `./versions/JSight API/0.3.0.md`.

## :notebook_with_decorative_cover: &nbsp; Dependencies

The **JSight Schema** language specification does not depend on anything.

The **JSight API** language specification depends on the version of the JSight Schema language. This
dependency is listed in the JSight API language specification in the [section
“Dependences”](./versions/JSight%20API/JSight%20API%200.3.6.md#dependences). 

## :sunglasses: &nbsp; Contributing

Contributing is more than just developing a language specification. You can help the project in many
ways, and we will be very happy to accept your contribution to our project.

Details of how you can help the project are described in the [CONTRIBUTING.md](./CONTRIBUTING.md)
document.

<div>
  &nbsp;
</div>

### Contributors

<a href="https://github.com/Emptyfruit"><img src="https://avatars.githubusercontent.com/u/14968783?v=4" width="100" height="100" alt=""/></a>
<a href="https://github.com/add2"><img src="https://avatars.githubusercontent.com/u/3954234?v=4" width="100" height="100" alt=""/></a>
<a href="https://github.com/constantine-malyshev"><img src="https://avatars.githubusercontent.com/u/101567029?v=4" width="100" height="100" alt=""/></a>

<div>  
  &nbsp; 
</div>

## :speech_balloon: &nbsp; Bugs and Feature Requests

Do you have a bug report or a feature request? 

Please feel free to add a [new
issue](https://github.com/jsightapi/specification/issues/new) or write to us in support:

- Email: [support@jsight.io](mailto:support@jsight.io)
- Telegram: [@jsight_support](https://t.me/jsight_support)

<div>  
  &nbsp; 
</div>

## :grey_question: &nbsp; Support

If something is unclear to you, please contact support; we try to respond within 24 hours. Moreover,
it is critical for us to understand what is unclear from the first instance.

- Email: [support@jsight.io](mailto:support@jsight.io)
- Telegram: [@jsight_support](https://t.me/jsight_support)

<div>  
  &nbsp; 
</div>

## :receipt: &nbsp; License

This project is licensed under the Apache 2.0 License. See the [LICENSE](./LICENSE) file for more
details.

<div>  
  &nbsp; 
</div>

## :book: &nbsp; Resources

- JSight Official Website: https://jsight.io.

### Documents:

- JSight API Language Quick Tutorial: https://jsight.io/docs/jsight-api-0-3-quick-tutorial.
- JSight Schema language specification on the official website:
  https://jsight.io/docs/jsight-schema-0-3.  
- JSight API language specification on the official website:
  https://jsight.io/docs/jsight-api-0-3.  

### Applications that use JSight API and JSight Schema:

- JSight Online Editor: https://editor.jsight.io.
  - JSight Online Editor repo: https://github.com/jsightapi/online-editor-frontend.
- JSight Server repo: https://github.com/jsightapi/jsight-server.

### Publications:

- JSight blog: https://jsight.io/blog.
- Official Facebook page: https://www.facebook.com/jsightapi.
- Official Twitter: https://twitter.com/jsightapi.
- Official Linkedin: https://www.linkedin.com/company/jsightapi.

### Others:

- All JSight repositories: https://github.com/jsightapi.
- GitHub discussions: https://github.com/jsightapi/specification/discussions.

<div>  
  &nbsp; 
</div>

## :handshake: &nbsp; Partners

- We have successfully completed [class #29 of the Alchemist
  Accelerator](https://www.alchemistaccelerator.com/portfolio?class=29).

<div>  
  &nbsp; 
</div>

## :trophy: &nbsp; Acknowledgments

We sincerely thank all those without whom this project would not have been possible:

- [Alchemist Accelerator](https://www.alchemistaccelerator.com/).

<div align="center">

:star: **Star us on GitHub — it motivates us a lot!** :star:

</div>
