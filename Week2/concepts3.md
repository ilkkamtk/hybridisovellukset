# GraphQL Core Concepts, pt 3

## Mutations

- **Definition**: Explain mutations as operations that modify data on a GraphQL server.
- **Syntax**: Introduce the syntax for writing mutations, including fields, input types, and variables.
- **Creating, Updating, Deleting**: Discuss how mutations are used for creating, updating, and deleting data.
- **Response Handling**: Explain how to handle responses from mutations and retrieve specific data after a mutation is
  performed.

---

## Input Types in GraphQL

In GraphQL, Input Types are special object types used specifically for passing complex data as arguments to mutations or
queries. They are similar to regular object types but are intended to represent input data rather than output data.

### Purpose

- **Data for Operations**: Input types are designed to encapsulate parameters or arguments for mutations and queries.
- **Structure**: They define the shape and structure of the data that can be passed as arguments.

### Characteristics

- **Fields**: Similar to regular object types, input types consist of fields that can hold scalar types or other input
  types.
- **Non-Nullable Fields**: Fields within input types can also be marked as non-nullable (`!`) to indicate mandatory
  values.
- **No Resolvers**: Unlike object types, input types don't have resolver functions associated with them, as they
  represent input data.

### Usage in Mutations and Queries

- **Mutation Arguments**: Input types are commonly used as arguments for mutations, allowing clients to send complex
  data for creating, updating, or deleting records.
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

## Variables in queries and mutations

- Variables are a way to pass dynamic values to queries and mutations.
- They are defined in the query or mutation and can be used multiple times.
- They are passed as a JSON object in the query or mutation request.
- Example:
  ```JavaScript
  const query = `
    query GetPosts($limit: Int!, $offset: Int!, $search: String) {
      getPosts(limit: $limit, offset: $offset, search: $search) {
        id
        title
        content
      }
    }
  `;
  
  const variables = {
      limit: 10,
      offset: 0,
      search: 'GraphQL',
  };
  
  const response = await fetch('http://localhost:3000/graphql', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        query,
        variables,
      }),
    });
  ```
    - The `$limit`, `$offset`, and `$search` variables are defined in the query.
    - The value of the variable is passed as a JSON object in the query request.
    - The response will contain the requested number of posts that match the search term.
    - Note that the variables are defined in the query, not the schema.
    - The variable names must be prefixed with `$` and the types must be specified in the query.
- Using variables makes the front-end development a bit easier, because you pass the variables as an object. Especially when you have multiple variables, string concatenation can become a bit messy:
  ```JavaScript
  const variables = {
      limit: 10,
      offset: 0,
      search: 'GraphQL',
  };
  
  const query = `
      query GetPosts($limit: Int!, $offset: Int!, $search: String) {
      getPosts(limit: ${variables.limit}, offset: ${variables.offset}, search: ${variables.search}) {
          id
          title
          content
      }
    }
  `;
  ```
