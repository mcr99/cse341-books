## Feature 2: Author CRUD Operations

### Goal
Create an `authors` collection to store author information and allow clients to perform all CRUD operations. Every author route must be documented and testable in Swagger.

### Data Model
Author documents will be stored in the `authors` collection.

Required author fields:
- `id`: string, required, custom id such as `a1`
- `name`: string, required
- `birthYear`: number, required, must be a positive integer and cannot be greater than the current year

Authors will use custom string ids instead of MongoDB `_id` values for route parameters.

### Routes

#### GET /authors
Purpose: Return all authors.

Success:
- Status code: `200`
- Response body: an array of author objects

Errors:
- `500` if an unexpected server or database error occurs

#### GET /authors/:id
Purpose: Return one author by its custom id.

Success:
- Status code: `200`
- Response body: the matching author object

Errors:
- `404` if no author exists with that id
- `500` if an unexpected server or database error occurs

#### POST /authors
Purpose: Create a new author.

Request body:

    {
      "id": "a2",
      "name": "Mary Johnson",
      "birthYear": 1990
    }

Success:
- Status code: `201`
- Response body: the newly created author object

Errors:
- `400` if a required field is missing
- `400` if the id already exists
- `400` if the birthYear is not a positive integer or is greater than the current year
- `500` if an unexpected server or database error occurs

#### PUT /authors/:id
Purpose: Update an existing author.

Request body:

    {
      "name": "Mary Johnson Updated",
      "birthYear": 1991
    }

Success:
- Status code: `200`
- Response body: the updated author object

Errors:
- `400` if a required field is missing
- `400` if the birthYear is not a positive integer or is greater than the current year
- `404` if no author exists with that id
- `500` if an unexpected server or database error occurs

The author's custom id cannot be changed during an update.

#### DELETE /authors/:id
Purpose: Delete an existing author.

Success:
- Status code: `204`
- Response body: none

Errors:
- `404` if no author exists with that id
- `409` if the author still has books associated with their id
- `500` if an unexpected server or database error occurs

### Relationship to Books
Before deleting an author, the API must check if any books use that author's id in the `authorId` field.

If books are associated with the author, the API must reject the deletion with a `409` status code. The books must be reassigned to another author or deleted before the author can be removed.

### Swagger Documentation
Swagger must document every author route, including request bodies, parameters, response status codes, and response examples. Every route must be testable from the browser.


# Books API Week 02 Spec - Version 1

## Feature 1: Book CRUD Operations and Author References

### Goal
Update the existing Week 01 book API so book documents include a reference to an author and the API supports all CRUD operations for books. Every book route must be documented and testable in Swagger.

### Data Model
Book documents will be stored in the `books` collection.

Required book fields:
- `id`: string, required, custom id such as `b1`
- `authorId`: string, required, references the `id` field of an author document
- `title`: string, required
- `publicationDate`: string, required

Books will continue to use custom string ids instead of MongoDB `_id` values for route parameters.

### Relationship to Authors
Each book will identify its author with an `authorId` field. The value of `authorId` must match the custom `id` value of an existing author document.

When creating or updating a book, the API should reject the request with a `400` status code if the submitted `authorId` does not match an existing author.

### Routes

#### GET /books
Purpose: Return all books.

Success:
- Status code: `200`
- Response body: an array of book objects

Errors:
- `500` if an unexpected server or database error occurs

#### GET /books/:id
Purpose: Return one book by its custom id.

Success:
- Status code: `200`
- Response body: the matching book object

Errors:
- `404` if no book exists with that id
- `500` if an unexpected server or database error occurs

#### POST /books
Purpose: Create a new book.

Request body:

    {
      "id": "b4",
      "authorId": "a1",
      "title": "Example Book Title",
      "publicationDate": "2026-01-15"
    }

Success:
- Status code: `201`
- Response body: the newly created book object

Errors:
- `400` if a required field is missing
- `400` if the `id` already exists
- `400` if the `authorId` does not match an existing author
- `500` if an unexpected server or database error occurs

#### PUT /books/:id
Purpose: Update an existing book.

Request body:

    {
      "authorId": "a2",
      "title": "Updated Book Title",
      "publicationDate": "2026-02-20"
    }

Success:
- Status code: `200`
- Response body: the updated book object

Errors:
- `400` if a required field is missing
- `400` if the `authorId` does not match an existing author
- `404` if no book exists with that id
- `500` if an unexpected server or database error occurs

#### DELETE /books/:id
Purpose: Delete an existing book.

Success:
- Status code: `204`
- Response body: none

Errors:
- `404` if no book exists with that id
- `500` if an unexpected server or database error occurs

### Swagger Documentation
Swagger must document every book route.

### Deployment Expectations
After implementation, the book routes must work locally and from the deployed Render application. The deployed Swagger page at `/api-docs` must allow someone to test every book route from the browser.

## Feature 2: Author CRUD Operations

TODO: Follow the example above to complete a spec for the authors feature.








# Books API Week 02 Spec - Version 2

## Feature 1: Book CRUD Operations and Author References

### Goal
Update the existing Week 01 Books API so book documents include a reference to an author and the API supports all CRUD operations for books. Every book route must be documented and testable in Swagger.

### Data Model
Book documents will be stored in the `books` collection.

Required book fields:
- `id`: string, required, custom id such as `b1`
- `authorId`: string, required, references the `id` field of an existing author document
- `title`: string, required
- `publicationDate`: string, required, must use the `YYYY-MM-DD` format and represent a valid date

Books will continue to use custom string ids instead of MongoDB `_id` values for route parameters.

The `id` field must be unique. A unique database index must be created for this field.

An index should also be created for `authorId` to improve searches for books by author.

### Relationship to Authors
Each book will identify its author with an `authorId` field. The value of `authorId` must match the custom `id` value of an existing author document.

When creating or updating a book, the API must check if the author exists.

The API must reject the request with a `400` status code if the submitted `authorId` does not match an existing author.

### Routes

#### GET /books
Purpose: Return all books.

Success:
- Status code: `200`
- Response body: an array of book objects
- If no books exist, return an empty array.

Errors:
- `500` if an unexpected server or database error occurs

#### GET /books/:id
Purpose: Return one book by its custom id.

Success:
- Status code: `200`
- Response body: the matching book object

Errors:
- `404` if no book exists with that id
- `500` if an unexpected server or database error occurs

#### POST /books
Purpose: Create a new book.

Request body:

    {
      "id": "b4",
      "authorId": "a1",
      "title": "Example Book Title",
      "publicationDate": "2026-01-15"
    }

Success:
- Status code: `201`
- Response body: the newly created book object

Errors:
- `400` if a required field is missing or invalid
- `400` if the id already exists
- `400` if the authorId does not match an existing author
- `500` if an unexpected server or database error occurs

### PUT /books/:id
Purpose: Update an existing book.

Request body:

    {
      "authorId": "a2",
      "title": "Updated Book Title",
      "publicationDate": "2026-02-20"
    }

Success:
- Status code: `200`
- Response body: the updated book object

Errors:
- `400` if a required field is missing or invalid
- `400` if the authorId does not match an existing author
- `404` if no book exists with that id
- `500` if an unexpected server or database error occurs

The book id cannot be changed during an update. The id must come from the URL parameter.

All editable fields are required in the PUT request body.

#### DELETE /books/:id
Purpose: Delete an existing book.

Success:
- Status code: `204`
- Response body: none

Errors:
- `404` if no book exists with that id
- `500` if an unexpected server or database error occurs

### Data Validation
- All required fields must be present.
- String fields cannot contain only empty spaces.
- The title must be trimmed to remove unnecessary spaces at the beginning and end.
- The authorId must reference an existing author.
- The publicationDate must use the `YYYY-MM-DD` format and represent a real calendar date.

### Error Responses
All error responses must use the same JSON structure:

    {
      "success": false,
      "message": "Description of the error."
    }

The API must not expose internal database information or sensitive server details in error messages.

### Swagger Documentation
Swagger must document every book route, including:
- HTTP method and endpoint.
- Route parameters.
- Required request body fields.
- Request body examples.
- Success response codes and examples.
- Error response codes and examples.

Every book route must be testable from the Swagger page.

### Deployment Expectations
After implementation, the book routes must work locally and from the deployed Render application.

The deployed Swagger page at `/api-docs` must allow someone to test every book route from the browser.

---

## Feature 2: Author CRUD Operations

### Goal
Create an authors collection to store author information and allow clients to perform all CRUD operations. Every author route must be documented and testable in Swagger.

### Data Model
Author documents will be stored in the `authors` collection.

Required author fields:
- `id`: string, required, custom id such as `a1`
- `name`: string, required, cannot be empty or contain only spaces
- `birthYear`: number, required, must be a positive integer and cannot be greater than the current year

Authors will use custom string ids instead of MongoDB `_id` values for route parameters.

The `id` field must be unique. A unique database index must be created for this field.

The name must be trimmed to remove unnecessary spaces at the beginning and end.

### Routes

#### GET /authors
Purpose: Return all authors.

Success:
- Status code: `200`
- Response body: an array of author objects
- If no authors exist, return an empty array.

Errors:
- `500` if an unexpected server or database error occurs

#### GET /authors/:id
Purpose: Return one author by its custom id.

Success:
- Status code: `200`
- Response body: the matching author object

Errors:
- `404` if no author exists with that id
- `500` if an unexpected server or database error occurs

#### POST /authors
Purpose: Create a new author.

Request body:

    {
      "id": "a2",
      "name": "Mary Johnson",
      "birthYear": 1990
    }

Success:
- Status code: `201`
- Response body: the newly created author object

Errors:
- `400` if a required field is missing or invalid
- `400` if the id already exists
- `400` if the name is empty or contains only spaces
- `400` if the birthYear is not a positive integer or is greater than the current year
- `500` if an unexpected server or database error occurs

#### PUT /authors/:id
Purpose: Update an existing author.

Request body:

    {
      "name": "Mary Johnson Updated",
      "birthYear": 1991
    }

Success:
- Status code: `200`
- Response body: the updated author object

Errors:
- `400` if a required field is missing or invalid
- `400` if the name is empty or contains only spaces
- `400` if the birthYear is not a positive integer or is greater than the current year
- `404` if no author exists with that id
- `500` if an unexpected server or database error occurs

The author's custom id cannot be changed during an update. The id must come from the URL parameter.

All editable fields are required in the PUT request body.

#### DELETE /authors/:id
Purpose: Delete an existing author.

Success:
- Status code: `204`
- Response body: none

Errors:
- `404` if no author exists with that id
- `409` if the author still has books associated with their id
- `500` if an unexpected server or database error occurs

### Relationship to Books
Before deleting an author, the API must check if any books use that author's id in the `authorId` field.

If books are associated with the author, the API must reject the deletion with a `409` status code.

The books must be reassigned to another existing author or deleted before the author can be removed.

### Error Responses
All error responses must use the same JSON structure:

    {
      "success": false,
      "message": "Description of the error."
    }

The API must not expose internal database information or sensitive server details in error messages.

### Swagger Documentation
Swagger must document every author route, including:
- HTTP method and endpoint.
- Route parameters.
- Required request body fields.
- Request body examples.
- Success response codes and examples.
- Error response codes and examples.

Every author route must be testable from the Swagger page.

### Deployment Expectations
After implementation, the author routes must work locally and from the deployed Render application.

The deployed Swagger page at `/api-docs` must allow someone to test every author route from the browser.

---

## General API Requirements

- Books and authors must use custom string ids.
- Custom ids must be unique in their respective collections.
- All incoming data must be validated before saving it to the database.
- All error responses must use a consistent JSON structure.
- Unexpected server and database errors must return status code `500`.
- Every API route must have complete Swagger documentation.
- Both collections must work locally and on the deployed Render application.