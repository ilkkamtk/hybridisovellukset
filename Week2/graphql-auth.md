## Authentication in GraphQL

1. [In Apollo authentication](https://www.apollographql.com/docs/apollo-server/security/authentication/) is done by adding user data to [context](https://www.apollographql.com/docs/apollo-server/data/resolvers/#the-contextvalue-argument)
2. [Example with Express.js](https://www.apollographql.com/docs/apollo-server/api/express-middleware/#context)

## Apollo Server Context
- Context is an object that is shared by all resolvers and is created for each request
- Context can be used to store user data for authentication and authorization.
- When using TypeScript, you need to define the type of context:
    ```typescript
    type MyContext = {
        user: User;
    };
    ```


## Step 1 - Add handling users to GraphQL server
1. Clone [authentication server](https://github.com/ilkkamtk/hybrid-auth-server.git) as a separate project.
2. Install dependencies and save `.env.sample` as `.env` and add your own values to the file. Remeber to use the same `JWT_SECRET` in all servers.
3. Start the server with `npm run dev`
4. Test the server with [Postman](https://www.postman.com/) or similar app. Create a new POST request to `http://localhost:3001/api/v1/auth/login` with the following body:
    ```json
    {
        "username": "xxxxx",
        "password": "yyyyy"
    }
    ```
    - You should get a response with a token.
    - Leave the server running.

5. Go to your **GraphQL project** and create a new branch `auth`
6. Create a new schema file `user.graphql` to `src/api/schemas` folder and add the following: 
    ```graphql
    type User {
        user_id: ID!
        username: String!
        email: String!
        level_name: String!
        created_at: String!
    }
    ```

7. Create a new resolver file `userResolver.ts` to `src/api/resolvers` folder. Add resolvers to get all users and a single user by id. This time we are making requests to a REST API instead of a database. So instead of using functions from the model files we are going to use the `fetchData()` function in `src/lib/functions.ts`.
   - Remember to update `resolvers/index.ts`.
8. Add a new query to `user.graphql`:
    ```graphql
    type Query {
        users: [User]
        user(user_id: ID!): User
    }
    ```
9. Add the new resolvers to `userResolver.ts`:
    ```typescript
    users: async () => {
        const users = await fetchData<UserWithNoPassword[]>(process.env.AUTH_SERVER + '/users');
        return users;
    },
    user: async (_parent: undefined, args: {user_id: string}) => {
        const user = await fetchData<UserWithNoPassword>(process.env.AUTH_SERVER + '/users/' + args.user_id);
        return user;
    },
    ```
10. Test the server with Sandbox. Create a new query:
     ```graphql
     query {
         users {
             user_id
             username
             email
             level_name
             created_at
         }
     }
     ```
     - You should get a response with all the users in the database.
     - Test also the `user` query to get a single user by id.
    
11. Create a new mutation for adding a new user. Modify the `user.graphql` and `userResolver.ts` files to add the necessary code. Don't forget to add an input type for the new user.
12. Test the mutation in Sandbox. Create a new mutation:
    ```graphql
    mutation CreateUser($input: UserInput!) {
        createUser(input: $input) {
            user_id
            username
            email
            level_name
            created_at
        }
    }
    ```
    - Add variables:
    ```json
    {
        "input": {
            "username": "xxxxx",
            "password": "yyyyy",
            "email": "zzzzz"
        }
    }
    ```
    - You should get a response with the newly created user.

## Step 2 - Add authentication
1. Install `jsonwebtoken` package: `npm i jsonwebtoken`
2. Create a new type for context. Add the following to `src/local-types/index.ts`:
     ```typescript
     import {UserWithLevel} from '@sharedTypes/DBTypes';
   
     type UserFromToken = Pick<UserWithLevel, 'user_id' | 'level_name'> & {
       token: string;
     };


     type MyContext = {
         user?: UserFromToken;
     };
   
     export type {MyContext, UserFromToken};
     ```
    - Note that user is optional because it is not available when the user is not authenticated.
3. Add the following to `src/api/index.ts`:
    ```typescript
    ...
    import {MyContext} from '../local-types';
    import {authenticate} from './lib/functions';

    const server = new ApolloServer<MyContext>({
        ...
    });
   
    app.use(
      '/graphql',
      cors<cors.CorsRequest>(),
      express.json(),
      expressMiddleware(server, {
        context: ({req}) => authenticate(req),
      })
    );
    ```
   - We are using the `authenticate` function to get the user data from the token and add it to context. Note the object destructuring with `{req}`. Context functions incoming arguments include `req` and `res` objects from Express.js.
4. Add the following to `src/lib/functions.ts`:
    ```typescript
    import jwt from 'jsonwebtoken';
    import jwt from 'jsonwebtoken';
    import {MyContext, UserFromToken} from '../local-types';
    import {Request} from 'express';
    import {GraphQLError} from 'graphql';
    
    ...
    
    export const authenticate = (req: Request): Promise<MyContext> => {
        const authHeader = req.headers.authorization;
        if (authHeader) {
            const token = authHeader.split(' ')[1];
            try {
                const user = jwt.verify(
                    token,
                    process.env.JWT_SECRET as string,
                   ) as UserFromToken;
                // add token to user object so we can use it in resolvers
                user.token = token;
                return {user};
            } catch (error) {
                throw new GraphQLError('Invalid/Expired token', {
                    extensions: {code: 'NOT_AUTHORIZED'},
                });
            }
        }
        return {};
    };
   
    export {fetchData, authenticate};
    ```
   - If there is no authentication header, we return an empty object. Otherwise we verify the token and return the user data. If token is invalid or expired, we throw a [GraphQLError](https://www.apollographql.com/docs/apollo-server/data/errors) with the code `NOT_AUTHORIZED`.

5. To check that the authentication works, add the following to `src/api/resolvers/mediaResolver.ts`:
    ```typescript
    createMediaItem: async (_parent: undefined, args: {input: Omit<MediaItem, 'media_id' | 'created_at' | 'thumbnail'>}, context: MyContext) => {
        console.log(context);
        return await postMedia(args.input);
    },
    ```
    - Test the mutation in Sandbox without adding a token to the request. You should get an empty object in the console.
    - Add a token to the request and test again. You should get the user data in the console. Use Postman to log in like in step 1 to get a token. Add `Authorization` header with value `Bearer <token>` to the headers tab in the bottom of the Sandbox. You should get the user data in the console.
6. To make the actual authentication add the following to `src/api/resolvers/mediaResolver.ts`:
    ```typescript
    createMediaItem: async (_parent: undefined, args: {input: Omit<MediaItem, 'media_id' | 'created_at' | 'thumbnail'>}, context: MyContext) => {
        if (!context.user || !context.user.user_id) {
            throw new GraphQLError('Not authorized', {
                extensions: {code: 'NOT_AUTHORIZED'},
            });
        }
        return await postMedia(args.input);
    },
    ```
   - The `authenticate` function already throws an error if the token is invalid or expired. Now we are kind of double-checking if the user data is available in context. If not, we throw a new error with the code `NOT_AUTHORIZED`.
   - Test the mutation in Sandbox without adding a token to the request. You should get an error with the code `NOT_AUTHORIZED`.
   - Add a token to the request and test again. You should get the newly created media item.
   - To authenticate the desired mutations and queries, add the same if-statement to the resolvers. Remember to add the context parameter to the resolver functions.
   - To differentiate user levels use `contex.user.user_level` in the if-statement. For example:
        ```typescript
        if (!context.user || context.user.user_level !== 'admin') {
            throw new GraphQLError('Not authorized', {
                extensions: {code: 'NOT_AUTHORIZED'},
            });
        }
        ```
     - You can also limit the properties that are sent to the client. For example:
        ```typescript
        if (context.user.user_level === 'admin') {
           return user;
        }
        return {
             user_id: user.user_id,
             username: user.username,
             email: undefined,
             level_name: undefined,
             created_at: undefined,
          };
          ```       
## Step 3 - Add login
1. Add the following to `src/api/schemas/user.graphql`:
    ```graphql
    type Mutation {
        login(username: String!, password: String!): LoginResponse
    }
    ```
2. Create a new type `LoginResponse` to `src/api/schemas/user.graphql` based on the response from the authentication server.
2. Add the following to `src/api/resolvers/userResolver.ts`:
    ```typescript
    login: async (_parent: undefined, args: Pick<User, 'username' | 'password'>) => {
        const options = {
            method: 'POST',
            headers: {'Content-Type': 'application/json'},
            body: JSON.stringify(args),
        };
        const user = await fetchData<UserWithNoPassword>(
            process.env.AUTH_SERVER + '/auth/login', options
            );
        return user;
    },
    ```
   - Since a login operation should be implemented as a mutation because it changes the state on the server (for example, it might create a new session, update the last login time for a user, or generate a new authentication token). 
4. Test the mutation in Sandbox. Create a new mutation:
    ```graphql
    mutation Login($username: String!, $password: String!) {
        login(username: $username, password: $password) {
            token
            user {
                user_id
                username
                email
                level_name
                created_at
            }
        }
    }
    ```
   - Add variables:
    ```json
    {
        "username": "xxxxx",
        "password": "yyyyy"
    }
    ```
   - You should get a response with a token and user data.

## Step 4 - Add owner to media item
1. Add the following to `src/api/schemas/media-item.graphql`:
    ```graphql
    type MediaType {
        ...
        owner: User
    }
    ```
2. Modify `userResolver.ts` to populate the `owner` property. Use `tagResolver.ts` as a reference.

## Step 5 - List of queries and mutations
Try to implement the following queries and mutations (some are already done). You can use the existing code as a reference. Remember to add the necessary code to the schema and resolver files. You can also add new files if necessary.
1. Queries
    - `users: [User]`
    - `user(user_id: ID!): User`
    - `checkToken: User`
    - `checkEmail(email: String!): Boolean`
    - `checkUsername(username: String!): Boolean`
    - `mediaItems: [MediaType]`
    - `mediaItem(media_id: ID!): MediaType`
    - `mediaItemsByTag(tag_id: ID!): [MediaType]`
    - `tags: [Tag]`
    - `tag(tag_id: ID!): Tag`
2. Mutations
    - `register(input: UserInput!): User`
    - `login(username: String!, password: String!): LoginResponse`
    - `updateUser(input: UserInput!): User`, get user_id from token
    - `deleteUser: Boolean`, get user_id from token
    - `updateUserAsAdmin(user_id: ID!, input: UserInput!): User`
    - `deleteUserAsAdmin(user_id: ID!): Boolean`
    - `createMediaItem(input: MediaItemInput!): MediaType`
    - `updateMediaItem(input: MediaItemInput!): MediaType`, user must be admin or the owner of the media item
    - `deleteMediaItem(media_id: ID!): Boolean`, user must be admin or the owner of the media item
    - `createTag(input: TagInput!): Tag`
    - `updateTag(input: TagInput!): Tag`
    - `deleteTag(tag_id: ID!): Boolean`, user must be admin
    - `addTagToMediaItem(media_id: ID!, tag_id: ID!): Boolean`, user must be admin or the owner of the media item
    - `removeTagFromMediaItem(media_id: ID!, tag_id: ID!): Boolean`, user must be admin or the owner of the media item

Commit and push your changes to GitHub.

