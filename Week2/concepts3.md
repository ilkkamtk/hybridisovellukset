# GraphQL Core Concepts, pt 3

## Mutations

- **Definition**: Explain mutations as operations that modify data on a GraphQL server.
- **Syntax**: Introduce the syntax for writing mutations, including fields, input types, and variables.
- **Creating, Updating, Deleting**: Discuss how mutations are used for creating, updating, and deleting data.
- **Response Handling**: Explain how to handle responses from mutations and retrieve specific data after a mutation is
  performed.

---

## Input Types in GraphQL

In GraphQL, Input Types are special object types used specifically for passing complex data as arguments to mutations or queries. They are similar to regular object types but are intended to represent input data rather than output data.

### Purpose
- **Data for Operations**: Input types are designed to encapsulate parameters or arguments for mutations and queries.
- **Structure**: They define the shape and structure of the data that can be passed as arguments.

### Characteristics
- **Fields**: Similar to regular object types, input types consist of fields that can hold scalar types or other input types.
- **Non-Nullable Fields**: Fields within input types can also be marked as non-nullable (`!`) to indicate mandatory values.
- **No Resolvers**: Unlike object types, input types don't have resolver functions associated with them, as they represent input data.

### Usage in Mutations and Queries
- **Mutation Arguments**: Input types are commonly used as arguments for mutations, allowing clients to send complex data for creating, updating, or deleting records.
- **Query Variables**: They can also be used with query variables to provide dynamic data in GraphQL queries.

### Example:
```graphql
input PostInput {
  title: String!
  content: String!
  authorId: ID!
}

type Mutation {
  createPost(input: PostInput!): Post
}
```

In this example:
- `PostInput` is an input type representing the structure of data required to create a post.
- The `createPost` mutation takes a `PostInput` object as an argument to create a new post.

---
