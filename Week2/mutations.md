# Mutations

## Step 1 - Add a new mutation to create a new media item

1. Create new branch `mutations`.
2. Add new mutation to `media-item.graphql`:
    ```graphql
    type Mutation {
      createMediaItem(input: MediaItemInput!): MediaItem
    }
    ```
3. Note that we cannot use `MediaItem` as type, for mutations we need to create an input type. Add new input type
   to `media-item.graphql`. The input type is based on `postMedia` function in `mediaModel.ts`:
    ```graphql
    input MediaItemInput {
      user_id: ID!
      title: String!
      etc...
    }
    ```
4. Add new resolver to `mediaResolver.ts`:
    ```typescript
    Mutation: {
      createMediaItem: async (_parent: undefined, args: {input: SomeType}) => {
        // call postMedia function from mediaModel.ts and return the result
      },
    },
    ```

5. Test the mutation in Sandbox:
    ```graphql
    mutation CreateMediaItem($input: MediaItemInput!) {
      createMediaItem(input: $input) {
      created_at
      description
      filename
      filesize
      media_id
      title
      }
    }
    ```
    - Add variables:
      ```json
      {
        "input": {
          "title": "Test",
          "description": "Test",
          "filename": "Test",
          "filesize": 1,
          "media_type": "Test",
          "user_id": someExistingUserId
        }
      }
      ```
    - You should get a response with the newly created media item.

## Step 2 - Add a new mutation to add a tag

1. Add new mutation to `tag.graphql`:
    ```graphql
    type Mutation {
      createTag(input: TagInput!): Tag
    }
    ```
2. Add new input type to `tag.graphql` where you define tag name.
3. Add new mutation to `tagResolver.ts` where you call `postTag` function from `tagModel.ts` and return the result.

4. Test the mutation in Sandbox:
    ```graphql
    mutation CreateTag($input: TagInput!) {
      createTag(input: $input) {
        tag_id
        tag_name
      }
    }
    ```
    - Add variables:
      ```json
      {
        "input": {
          "tag_name": "Test"
        }
      }
      ```
    - You should get a response with the newly created tag.

## Step 3 - Add a new mutation to add a tag to a media item
1. Add new mutation to `media-item.graphql`:
    ```graphql
      addTagToMediaItem(input: AddTagToMediaItemInput!): MediaItem
    ```
   - The reason why this is in `media-item.graphql` (and `mediaResolver`) and not in `tag.graphql` is because return type is `MediaItem`.
2. Add a new input type `AddTagToMediaItemInput` to `media-item.graphql` where you define `media_id` and `tag_name`.
3. Add new resolver to `mediaResolver.ts` where you call `postTagToMediaItem` function from `mediaModel.ts` and return the result.
   - Capitalize the first letter of tag_name before you send it to `postTagToMediaItem()`because we want all tags to be the same format in the database, so we can query them and check for duplicates more easily without having to worry about case sensitivity.

## Step 4 - Add a new mutation to delete a tag
1. Add new mutation to `tag.graphql`:
    ```graphql
    deleteTag(input: ID!): Message
    ```
2. Note that the return type is Message. Create new file `messages.graphql` to `src/api/schemas` folder and add the following:
    ```graphql
    type Message {
      message: String!
    }
    ```
3. Add new mutation to `tagResolver.ts` where you call `deleteTag` function from `tagModel.ts` and return the result.
4. Test the mutation in Sandbox:
    ```graphql
    mutation DeleteTag($input: ID!) {
      deleteTag(input: $input) {
        tag_id
        tag_name
      }
    }
    ```
    - Add variables:
      ```json
      {
        "input": "tag_id"
      }
      ```
    - You should get a response with the deleted tag.

## Step 5 - Add a new mutation to delete a media item
1. Use the same steps as in Step 4 to add a new mutation to delete a media item.

## Step 6 - Add a new mutation to update a media item
1. Add new mutation to `media-item.graphql`:
    ```graphql
    updateMediaItem(input: MediaItemInput!): MediaItem
    ```
2. Add new resolver to `mediaResolver.ts` where you call `putMedia` function from `mediaModel.ts` and return the result.
   - Note the return type of `putMedia` and consider adding a new type to `messages.graphql`.
3. Test the mutation in Sandbox:
    ```graphql
    mutation UpdateMediaItem($input: MediaItemInput!) {
      updateMediaItem(input: $input) {
        media_id
        title
        description
      }
    }
    ```
    - Add variables:
      ```json
      {
        "input": {
          "media_id": "media_id",
          "title": "Test",
          "description": "Test"
        }
      }
      ```
    - You should get a response with the updated media item.

## Step 7 - Fixes?
- Consider which properties in the GraphQL schemas should be required and which should be optional by using the `!` non-null assertion operator. 
- Also consider do you need to extra input types for mutations or can you use the existing ones.
- Are the return types of all the mutations correct?

Commit and push your changes to GitHub.
