# Apollo Server Setup

## Apollo Server

- [Apollo Server](https://www.apollographql.com/docs/apollo-server/) is an open-source GraphQL server that is compatible
  with any GraphQL client.
- It's built with Node.js and can be used with any database or data source including third-party REST APIs.
- These instructions apply to version 4.10.0.

## Step 1 - Project Boilerplate

1. Clone or download as ZIP the starter project from [GitHub](https://github.com/ilkkamtk/hybrid-graphql-starter).
2. Rename the project folder to `hybrid-graphql-server`.
3. Enter the project folder and run `npm i` to install the dependencies.
4. (Create a new repository in GitHub and push the project to it.)
5. Run `npm run dev` to start the server. Open `localhost:3000` in your browser to see if the server is running.
    - Run also `npm run test` to see if the tests are passing.
6. Open the project in VSCode.

## Step 2 - Install [Apollo Server with Express Middleware](https://www.apollographql.com/docs/apollo-server/api/express-middleware) and some [GraphQL tools](https://the-guild.dev/graphql/tools/docs/schema-merging#file-loading)

1. Install Apollo Server and its dependencies:
    - `npm i @apollo/server graphql`
    - `npm i @graphql-tools/load-files @graphql-tools/merge`
2. Create a new schema file `media-item.graphql` in the `src/api/schemas` folder.
3. Add a new type definition for `MediaItem` based on the return type of `fetchAllMedia` in `mediaModel.ts`:
   ```graphql
   type MediaItem {
     media_id: ID!
     etc...
   }
   
   type Query {
     mediaItems: [MediaType]
   }
   ```
4. Uncomment every line in `src/api/schemas/index.ts`.
5. Create a new resolver file `mediaResolver.ts` in the `src/api/resolvers` folder:
    ```typescript
    import {fetchAllMedia} from '../models/mediaModel';

    export default {
        Query: {
            mediaItems: async () => {
                return await fetchAllMedia();
            },
        },
    };
    ```
6. Add the following to `src/app.ts`:
    ```typescript
    ...
    import {ApolloServer} from 'apollo-server';
    import {loadFilesSync} from '@graphql-tools/load-files';
    import typeDefs from './api/schemas/index';
    import resolvers from './api/resolvers/index';
    ...
    // remove app.use(cors());
    // after app.get('/', (req....
    const server = new ApolloServer({
      typeDefs,
      resolvers,
    });

    await server.start();

    app.use('/graphql', cors(), express.json(), expressMiddleware(server));
    ...
    ```
7. Test the server with Postman. Create a new request; type: POST, url: `localhost:3000/graphql`.
    - Try the following query:
        ```graphql
        query {
          mediaItems {
            media_id
            title
            description
          }
        }
        ```
    - You should get a response with all the media items in the database.

## Step 3 - Add Sandbox for easier testing

1. Add the following import to `src/app.ts`:
    ```typescript
    import { 
       ApolloServerPluginLandingPageLocalDefault,
       ApolloServerPluginLandingPageProductionDefault,
    } from '@apollo/server/plugin/landingPage/default';
    ```

2. Modify the `server` variable in `src/app.ts`:
    ```typescript
    const server = new ApolloServer({
      typeDefs,
      resolvers,
      plugins: [
        ApolloServerPluginLandingPageLocalDefault(),
      ],
    });
    ```

3. Open `http://localhost:3000/graphql` in your browser. You should see the GraphQL Sandbox.
    - Try the following query in the sandbox:
         ```graphql
         query {
           mediaItems {
             media_id
             title
             description
           }
         }
         ```
4. On production, you can use the following configuration:
    ```typescript
    const server = new ApolloServer({
      typeDefs,
      resolvers,
      plugins: [
        ApolloServerPluginLandingPageProductionDefault(),
      ],
    });
    ```
5. You can also use ternary operator to use different plugins based on the environment:
    ```typescript
    const server = new ApolloServer({
      typeDefs,
      resolvers,
      plugins: [
        process.env.NODE_ENV === 'production'
          ? ApolloServerPluginLandingPageProductionDefault()
          : ApolloServerPluginLandingPageLocalDefault(),
      ],
    });
    ```

6. Commit and push your changes to GitHub.
