# GraphQL Core Concepts

## Schema

### Definition and Purpose

- **Foundation of API**: The schema acts as a contract between the client and the server, specifying how clients can
  request and manipulate data.
- **Type System**: It's built around a strongly typed system where types represent the shape of the data available in
  the API.

### SDL in GraphQL

SDL stands for "Schema Definition Language." In the context of GraphQL, SDL refers to the syntax used to define the
schema of a GraphQL API.

#### Schema Definition

- **Structured Language**: SDL provides a clear and human-readable way to define the structure of a GraphQL schema.
- **Defines Types and Relationships**: SDL allows developers to define types, their fields, relationships between types,
  and the available operations.

#### Basic Syntax

- **Type Definitions**: SDL uses a simple syntax to define types, including scalar types (e.g., String, Int), object
  types, input types, enums, interfaces, and unions.
- **Fields and Relationships**: Within types, fields are defined to represent the data points, and relationships between
  types are established.

### Types

- **Scalar Types**: Basic atomic types like integers, floats, strings, booleans, and
  custom-defined [scalars](https://gist.github.com/ilkkamtk/902bee49d16d108300b03f9d2f31cfef).
- **Object Types**: Composite types representing complex objects with fields. For instance, a `User` type might have
  fields like `name`, `email`, and `age`.
- **Input Types**: Used specifically for arguments in mutations to pass complex objects as arguments.

### Relationships and Fields

- **Fields**: Within types, fields define the data available for retrieval. They can be scalar types or reference other
  types.
- **Relationships**: Types can have relationships with other types, allowing for complex data structures and retrieval
  of interconnected data.

### Type Definitions and Resolvers

- **Type Definitions**: In a schema, type definitions specify the structure of types, their fields, and relationships.
- **Resolvers**: Functions that define how to fetch the data associated with a particular field. Resolvers retrieve the
  data from the appropriate data source.

### Schema Evolution

- **Versioning and Evolution**: The schema can evolve over time by adding new types or fields without breaking existing
  queries. This backward compatibility allows for smooth transitions and gradual improvements in the API.

### Example:

```graphql
type User {
  id: ID
  name: String
  email: String
}

type Post {
  id: ID
  title: String
  content: String
  author: User
}

type Query {
  getUser(id: ID): User
  getPost(id: ID): Post
  getPosts: [Post]
}
```

This simple schema defines `User` and `Post` types with their fields and a `Query` type that includes entry points
—basically functions— for fetching users and posts.

---

## Exclamation Point (`!`) in GraphQL

In GraphQL schemas, the exclamation point (`!`) is used to denote a field or a type as non-nullable, meaning that it must always have a value. Here's a breakdown of how it's used:

### Non-nullable Fields
- **Fields**: Adding `!` after a field's type declaration indicates that the field cannot be null. It must always return a value of that specified type.
  - Example: `name: String!` means that the `name` field in an object of this type will always be a non-null string.

### Non-nullable Types
- **Type Declarations**: It applies to the type declaration itself, ensuring that any instance of that type cannot be null.
  - Example: `author: User!` indicates that the `author` field within a `Post` type must always resolve to a non-null `User`.

### Significance
- **Enforced Presence**: Using `!` helps enforce the presence of data, ensuring that fields or types cannot return null values, reducing the chance of unexpected null responses.
- **Query Validation**: It guides clients by explicitly stating which fields are guaranteed to exist in the response, helping in query validation.

#### Example:
```graphql
type User {
  id: ID!
  name: String!
  email: String
}

type Post {
  id: ID!
  title: String!
  content: String!
  author: User!  # Non-nullable User type
}
```

In this schema:
- `name` and `id` fields in the `User` type are non-nullable.
- `title`, `content`, and `id` fields in the `Post` type are also non-nullable, and `author` is a non-nullable `User`.

---

## Queries

Queries in GraphQL are requests made by clients to retrieve specific data from a GraphQL API. They serve as a mechanism for clients to precisely specify the shape and content of the data they need.

### Customized Data Retrieval

- **Client-Specified Data**: Clients define the structure of the response by specifying the fields and relationships
  they want in the query.
- **Precise Requests**: Clients can request exactly the data they need, avoiding over-fetching or under-fetching common
  in traditional APIs.

### Syntax and Flexibility

- **Graphical Query Language**: Queries are written in a declarative and hierarchical syntax, allowing clients to
  navigate through different types and their relationships.
- **Nested Fields**: Clients can query nested fields to access related data across different types in a single request.

### Single Endpoint

- **Single Entry Point**: GraphQL typically uses a single endpoint for all queries, providing a unified interface for
  data retrieval.
- **Multiple Data Fetching**: Clients can request multiple resources or related data in one query.

### Example:

```graphql
# Example GraphQL Query
{
  getUser(id: "123") {
    name
    email
    posts {
      title
      content
    }
  }
}
```

In this query:

- `getUser` is the entry point to fetch a user by ID.
- Within the user object, `name` and `email` fields are requested.
- Additionally, the `posts` field within the user object retrieves specific fields (`title` and `content`) of the posts
  associated with that user.

---
