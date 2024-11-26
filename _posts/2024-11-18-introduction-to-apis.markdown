---
title:  "Design Components: APIs"
date:   2024-11-18 08:00:00 -0600
excerpt: "What is an API?"
---
## What is an API?
API stands for Application Programming Interface. What does this mean though? APIs are the means by which computer applications interact with external applications. 

### Example
Say I have an ecommerce shop online where I sell collectables. How am I going to ship my packages? This is a hard problem to solve. Instead of solving it myself, I'd rather let dedicated experts design a solution that I can use. This is where APIs come in. In this example, I can use the Shippo API to get shipping rates and labels automatically so I can ship my packages.
<figure>
    <a href="/assets/images/api_example.png"><img src="/assets/images/api_example.png"></a>
    <figcaption>A diagram showing a simple example where an ecommerce application calls the Shippo API in order to generate a shipping label. </figcaption>
</figure>

### What is an API Endpoint?
An endpoint in an API is the application that is responsible for serving a specific resource. Depending on the type of API, an API can have multiple endpoints. 

#### Example Endpoints
In the Shippo example, the endpoint where the user requests a shipping label is `/labels`. Shippo has other API endpoints as well, such as `/tracks` for tracking a package and `/shipments` for requesting shipping prices.

## Popular API Types

### REST
REST APIs communicate using the HTTP protocol. They use a data format called JSON in request bodies. REST APIs are the most common APIs.

Example JSON:

```json
{
    "id": 123,
    "title": "Introduction to APIs",
    "author": "Jane Doe",
    "publishedYear": 2024
}
```

### SOAP
SOAP APIs use the SOAP protocol to communicate (although they commonly use HTTP.) SOAP APIs use a data format called XML.

Example XML:

```xml
<Book>
    <id>123</id>
    <title>Introduction to APIs</title>
    <author>Jane Doe</author>
    <publishedYear>2024</publishedYear>
</Book>
```

### GraphQL
GraphQL APIs are a unique type of API that allows users to request what specific resources they want, all in one request. This is opposed to the REST API model, where one usually needs to make multiple different requests to get related resources.

Here's an example GraphQL query used to request metadata associated with a book:

```graphql
query {
  book(id: 123) {
    id
    title
    author
    publishedYear
  }
}
```

example response:

```json
{
  "data": {
    "book": {
      "id": 123,
      "title": "Introduction to APIs",
      "author": "Jane Doe",
      "publishedYear": 2024
    }
  }

```

## APIs Under the Hood
Typically, APIs are applications built on top of a database. The API then becomes a means by which to create or modify objects in the database. Let's look deeper at REST APIs to understand this idea better.

### CRUD
CRUD stands for **Create**, **Read**, **Update**, **Delete**. Each REST API implements each of these functionalities. CRUD is implemented by **REST verbs** on each endpoint. 

### REST Verbs
**REST Verbs** are the different methods that programs use to interact with REST APIs. They define the functionalities of each API endpoint. Each verb corresponds to an action on a database. Let's examine each REST verb and how it affects an underlying database.

1. GET
    - Purpose: Retrieves data from the database without modifying it.
    - Usage: Used for read-only operations to fetch information about resources.
    - Idempotence: Yes, `GET` requests are idempotent (calling it multiple times produces the same result).
    - Example:`GET /books`: Retrieves a list of all books. `GET /books/123`: Retrieves details of the book with ID 123.

2. POST
    - Purpose: Creates a new resource on the database.
    - Usage: Used when the client needs to add new data, such as creating a new record or entry.
    - Idempotence: No, `POST` is not idempotent (repeated calls create duplicate resources unless handled otherwise).
    - Example: `POST /books`: Creates a new book record with the data provided in the request body.

3. PUT
    - Purpose: Updates or replaces an existing resource on the database.
    - Usage: Typically used to replace the entire content of a resource. If the resource doesn’t exist, it can sometimes create it (though this depends on the API design).
    - Idempotence: Yes, `PUT` is idempotent (repeated calls with the same data result in the same resource state).
    - Example: `PUT /books/123`: Replaces the book with ID 123 with the data provided in the request body.

4. PATCH
    - Purpose: Partially updates an existing resource on the database.
    - Usage: Used to modify only specific fields of a resource without affecting other fields.
    - Idempotence: Yes, `PATCH` is idempotent if the same patch data is applied multiple times.
    - Example:`PATCH /books/123`: Updates only specified fields (e.g., title or author) of the book with ID 123.

5. DELETE
    - Purpose: Deletes a resource from the database.
    - Usage: Used when the client needs to remove a resource.
    - Idempotence: Yes, `DELETE` is idempotent (repeated calls result in the same resource state—deleted).
    - Example:`DELETE /books/123`: Deletes the book with ID 123.

## Key Takeaways
- APIs are applications that other applications can send requests to over a network. 
- Popular API types include REST, SOAP, and GraphQL.
- APIs are CRUD applications, meaning they're used to modify database resources. 




