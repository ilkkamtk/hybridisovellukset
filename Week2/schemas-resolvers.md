# Add Schemas to your project

## Step 1 - Tag schema

1. Create a new schema file `tag.graphql` in the `src/api/schemas` folder.
2. Add a new type definition for `Tag` based on the return type of `fetchAllTags` in `tagModel.ts`:
    ```graphql
    type Tag {
      tag_id: ID
      etc...
    }

    type Query {
      tags: [Tag]
    }
    ```
3. Create a new resolver file `tagResolver.ts` in the `src/api/resolvers` folder:
    ```typescript
    import {fetchAllTags} from '../models/tagModel';

    export default {
        Query: {
            tags: async () => {
                return await fetchAllTags();
            },
        },
    };
    ```
4. Restart the server by typing `rs` in the terminal where the server is running.
5. Test the server with Sandbox. Create a new query:
    ```graphql
    query {
      tags {
        tag_id
        tag_name
        media_id
      }
    }
    ```
    - You should get a response with all the tags in the database.

## Step 2 - Get media by id
1. Add a new resolver to `mediaResolver.ts`:
    ```typescript
    mediaItem: async (_parent: undefined, args: {media_id: string}) => {
        return await fetchMediaById(args.media_id);
    },
    ```
2. Why in `args: {media_id: string}` `media_id`is of type `string` and not `number`?
3. Why is `args.media_id` causing type error? How to fix it?
4. Add a new query to `media-type.graphql`: `mediaItem(media_id: ID!): MediaType`.
5. Because changes to _graphql_ files don't invoke restart automatically, restart the server manually by typing `rs` in the terminal where the server is running.
6. Test the server with Sandbox. Create a new query:
    ```graphql
    query {
      mediaItem(media_id: X) {
        media_id
        title
        description
      }
    }
    ```
    - You should get a response with the media item with id X.

## Step 3 - Show all tags that media item has when getting all media items or a single media item
1. Add new property `tags` to `MediaType` in `media-type.graphql`:
    ```graphql
    type MediaType {
      ...
      tags: [Tag]
    }
    ```
2. To populate the `tags` property, add a new resolver to `tagResolver.ts`:
    ```typescript
    MediaItem: {
        tags: async (parent: {media_id: string}) => {
            return await fetchTagsByMediaId(parent.media_id);
        },
    },
    Query: {
        ...
    },
    ```
   - This might seem a bit counterintuitive. Why is the resolver for `MediaItem`  in `tagResolver.ts` and not in `mediaResolver.ts`? Because we are fetching tags, not media items. The resolver is in the file that is responsible for fetching tags. 
   - Why is it called `MediaItem`? Because that is the parent type of `tags` property.
3. Test the server with Sandbox. Create a new query:
    ```graphql
    query {
      mediaItems {
        media_id
        title
        description
        tags {
          tag_id
          tag_name
        }
      }
    }
    ```
    - You should get a response with all the media items and their tags.

## Step 4 - Get media items by tag
1. Add a new resolver to `mediaResolver.ts` where the input argument is `tag_id`. The argument is then passed to `fetchMediaItemsByTag` function.
2. Add a new query to `media-type.graphql`: `mediaItemsByTag(tag: String!): [MediaItem]`.



