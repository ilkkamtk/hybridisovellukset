# GraphQL Core Concepts, pt 2

## Type System in GraphQL

### Scalars

- **Definition**: Scalars are the atomic types in GraphQL that represent primitive data types like integers, floats,
  strings, booleans, and custom-defined scalars.
- **Single Value**: They hold individual data values without any substructure.
- GraphQL comes with a set of default scalar types out of the box:
  - **Int**: A signed 32‐bit integer. 
  - **Float**: A signed double-precision floating-point value. 
  - **String**: A UTF‐8 character sequence. 
  - **Boolean**: true or false. 
  - **ID**: The ID scalar type represents a unique identifier, often used to refetch an object or as the key for a cache. The ID type is serialized in the same way as a String; however, defining it as an ID signifies that it is not intended to be human‐readable.

### Objects

- **Complex Types**: Objects are composite types that allow grouping multiple fields together into a single unit.
- **Fields and Relationships**: Object types define fields representing various data points and can express
  relationships between different object types.

#### Interfaces and Unions

- **Interfaces**: They define a set of common fields that other object types can implement. They allow for creating
  shared fields across multiple types.
- **Unions**: Unions enable declaring a type that could be one of several specified object types. They provide
  flexibility when a field can return different types of objects.

#### Example:

```graphql
scalar Date

type User {
  id: ID!
  name: String!
  email: String
}

type Post {
  id: ID!
  title: String!
  content: String!
  author: User!
  createdDate: Date!
}

interface Character {
  id: ID!
  name: String!
}

type Human implements Character {
  id: ID!
  name: String!
  planet: String!
}

type Droid implements Character {
  id: ID!
  name: String!
  primaryFunction: String!
}

union SearchResult = Human | Droid

type Query {
  getUser(id: ID!): User
  getPost(id: ID!): Post
  getCharacter(id: ID!): Character
  search(query: String!): [SearchResult!]!
}
```

In this example:

- `Scalars` include `Date`, a custom scalar representing
  dates. [Read this.](https://gist.github.com/ilkkamtk/d70d0775a0a90f9824f5fe1238188020)
- `User` and `Post` are `Object` types with defined fields.
- `Character` is an `Interface` implemented by `Human` and `Droid` types, sharing common fields.
- `SearchResult` is a `Union` that could be either a `Human` or a `Droid`.

#### Example query

```graphql
# Example GraphQL Query
{
  getUser(id: "123") {
    id
    name
    email
  }
  
  getPost(id: "456") {
    id
    title
    content
    createdDate
    author {
      id
      name
    }
  }
  
  getCharacter(id: "789") {
    id
    name
    ... on Human {
      planet
    }
    ... on Droid {
      primaryFunction
    }
  }
  
  search(query: "Skywalker") {
    __typename
    ... on Human {
      id
      name
      planet
    }
    ... on Droid {
      id
      name
      primaryFunction
    }
  }
}
```

Explanation:

- `getUser` retrieves a user with ID "123" and specifies fields (`id`, `name`, `email`).
- `getPost` fetches a post with ID "456", including fields (`id`, `title`, `content`, `createdDate`), and the
  author's (`User`) ID and name.
- `getCharacter` returns a character (either `Human` or `Droid`) with ID "789", and based on its type, retrieves
  specific fields (`planet` for `Human`, `primaryFunction` for `Droid`).
- `search` searches for characters matching the query "Skywalker" and returns their respective IDs, names, and
  additional details based on their types (`Human` or `Droid`).

---

## Resolvers

Resolvers are functions responsible for populating the data for each* field in a GraphQL schema. Resolvers are used to
resolve GraphQL queries, mutations, and subscriptions by defining how to fetch or compute the data for each field in the
schema.

_*However, in GraphQL, even if every field in a schema has the potential for an associated resolver function, the
presence of a resolver isn't mandatory for every field, especially when using a GraphQL server that follows certain
conventions or defaults. Most of the time resolvers are made only for queries and mutations._

#### Resolver Map

- **Structure**: Resolvers are typically organized within a resolver map, a JavaScript object that maps types and fields
  in the schema to resolver functions.
- **Field-Level Resolvers**: Each field in the schema that requires custom logic or data retrieval has a corresponding
  resolver function defined in the resolver map.

#### Structure of Resolvers in Apollo Server

- **Field Resolution**: Resolvers are functions that receive parent data (if applicable), arguments, context, and
  information about the GraphQL query (like AST nodes).
- **Data Fetching**: Resolvers interact with data sources (such as databases, REST APIs, or other services) to fetch or
  compute the required data for the associated field.

#### Example:

```graphql
type Animal {
  id: ID
  animal_name: String
  species: Int
}

type Query {
  animals: [Animal]
}
```

```javascript
const animalData = [
  {
    id: '1',
    animal_name: 'Frank',
    species: '1',
  },
  {
    id: '2',
    animal_name: 'James',
    species: '1',
  },
];

export default {
  Query: {
    animals: () => {
      return animalData;
    },
  },
};
```

In this example, we have a simple GraphQL schema and a JavaScript resolver implementation using Apollo Server.

When a GraphQL query is made for the `animals` field, the resolver for the `animals` query field is triggered. It
returns the `animalData` array containing two objects representing animals.

---
