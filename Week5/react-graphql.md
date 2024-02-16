# GraphQL with React
Previously we made a GraphQL server with Apollo Server. [Download the full version here](https://github.com/ilkkamtk/hybrid-graphql). Now we will use that server with a React application. We could use [Apollo Client,](https://www.apollographql.com/docs/react/) but we will use normal fetch for now.

## Using fetch() with GraphQL
When using fetch with GraphQL, we need to send a POST request with the query and possible variables in the body. The server will respond with a JSON object. The object will have a `data` property with the requested data or an `errors` property with an array of errors. The `data` that has the requested data is an object with the same structure as the query.

```tsx
const query = {
  query: `
    query {
      allPersons {
        name
        phone
        id
      }
    }
  `
};

const doFetch = async (query) => {
  const response = await fetch('http://localhost:4000/graphql', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({ query }),
  });
  const result = await response.json();
  console.log(result.data.allPersons);
};

doFetch(query);
```

## Lab assignment 1
1. Create a new branch `react-graphql` with git.
2. Download the latest version of the [GraphQL server](https://github.com/ilkkamtk/hybrid-graphql)
3. Also check if the [common types](https://github.com/ilkkamtk/hybrid-types) have been updated and need to be downloaded.
4. We need to use also the Rest API version at the same time when converting `ApiHooks.ts` to use GraphQL. So we need to set the GraphQL server to run on a different port. Change the port in `.env` file to `3003`.
5. Since using fetch with GraphQL is always the same, create a new generic function `makeQuery` to `src/lib/functions.ts`. The function should take a query (string), optional variables (Record), and an optional token (string) as parameters. The function should return a Promise of generic type that describes the fetched data. The function should call `fetchData` function so send a `POST` request to the server:
    ```tsx
    
     const makeQuery = async (query, variables, token) => {
        const headers = {
            'Content-Type': 'application/json',
            Authorization: `Bearer ${token}`,
        };
   
        const body = {
            query,
            variables,
        };
        const options: RequestInit = {
            method: 'POST',
            headers,
            body: JSON.stringify(body),
        };
        return await fetchData(
            import.meta.env.VITE_GRAPHQL_API as string,
            options,
        );
    }; 
    ```
   - You'll probably get some type errors. Fix them.
6. We need to create a generic type for the fetched data. Add the following type to `src/types/LocalTypes.ts`:
    ```tsx
    type GraphQLResponse<T> = {
        data: T;
        errors?: { message: string }[];
    };
    export { GraphQLResponse };
    ```
   - This type is a generic type that describes the response from the server. The `data` property is the requested data and the `errors` property is an array of errors.
   - When using this type, replace `T` with the type of the data you are expecting. For example with this query:
     ```tsx
     const query = `
       query {
         allPersons {
           name
           phone
           id
         }
       }
     `;
     ```
     The type of the `data` property is `AllPersons[]`. So the type of the `GraphQLResponse` is `GraphQLResponse<{ allPersons: AllPersons[] }>`.
 
7. Create a new file `GraphQLHooks.tsx` to `hooks` folder by copying `ApiHooks.tsx` and renaming it. Start converting the functions to use GraphQL. For example, the `getMedia` function in `useMedia` hook should like this:
    ```tsx
    const getMedia = async () => {
        try {
            const query = `
          query MediaItems {
            mediaItems {
              media_id
              user_id
              owner {
                username
                user_id
              }
              filename
              thumbnail
              filesize
              media_type
              title
              description
              created_at
              comments_count
              average_rating
            }
          }
        `;
    
            const result =
                await makeQuery<GraphQLResponse<{mediaItems: MediaItemWithOwner[]}>>(
                    query,
                );
            setMediaArray(result.data.mediaItems);
        } catch (e) {
            console.log('getMedia', (e as Error).message);
        }
    };
    ```
8. To use the new `GraphQLHooks.tsx` in the app, change the import in `Home.tsx`:
    ```tsx
    import { useMedia } from './hooks/GraphQLHooks';
    ```
9. Test the app. Check that the media list is still displayed.
   - Show `comments_count` in `MediaItemCard` component.
10. Practice using GraphQL by modifying `useUser` hook to get user data in `Profile.tsx`.
11. Test the app. Check that the user data is displayed.

## Lab assignment 2
1. Continue in the same branch.
2. Use the same `makeQuery` function to modify `postLogin` function in `useAuthentication` hook. The function should call the `login` mutation:
    ```tsx
    const postLogin = async (inputs: Record<string, string>) => {
        const query = `
            mutation Login($username: String!, $password: String!) {
              login(username: $username, password: $password) {
                token
                message
                user {
                  user_id
                  username
                  email
                  level_name
                  created_at
                }
              }
            }
          `;
        const loginResult = await makeQuery<LoginResponse>(query, inputs);
        return loginResult;
    };
    ```
3. Test that the login works.
4. Convert all the other hooks is `GraphQLHooks.tsx` to use GraphQL.

## Submit
1. Run `npm build` or `npm run build`
2. Move build folder to your public_html
3. Test your app: `http://users.metropolia.fi/~username/graphql`
4. Modify README.md. Change the link in `Open [X](X) to view it in the browser.` to point to the above link.
5. git add, commit & push to remote repository
6. Submit the link to correct branch of your repository to Oma
