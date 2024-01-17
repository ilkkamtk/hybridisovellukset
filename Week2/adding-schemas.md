# Add Schemas to your project

## Step 1
1. Create a new schema file `tag.graphql` in the `src/api/schemas` folder.
2. Add a new type definition for `Tag` based on the TypeScript type with the same name:
   ```graphql
   type Tag {
     tag_id: ID!
     tag_name: String!
   }
   
   type Query {
     tags: [Tag]
   }
   ```
