# Uploading with GraphQL

[Best practices](https://www.apollographql.com/blog/backend/file-uploads/file-upload-best-practices/)

## Step 1 - Upload server

1. Domnload [upload server](https://github.com/ilkkamtk/hybrid-upload-server.git) as Zip.
2. Install dependencies: `npm i`
3. Start server: `npm run dev`
4. Use your GraphQL server to log in and get a token
4. Test upload with [Postman](https://www.postman.com/downloads/)
    1. Create a new request
    2. Set request type to `POST`
    3. Set URL to `http://localhost:3002/api/v1/upload`
    4. Select `Authorization` tab
    5. Select `Bearer Token` and paste your token to the field
    6. Select `Body` tab
    7. Select `form-data`
    8. Add a new key `file` and select an image file to upload
    9. Send request
    10. Check that file was uploaded to `uploads` folder
    11. Check also that there is a thumbnail.
    12. Repeat with a video file
5. Keep server running

## Step 2 - Client
1. Clone the [client](https://github.com/ilkkamtk/hybrid-upload-client-starter) as a separate project.
2. Install dependencies: `npm i`
3. Start client: `npm run dev`
4. Open `http://localhost:5173/` in your browser (or whatever port you have configured in Vite)
5. At this point login and file listing should work. Log out by deleting the token from local storage in the browser.
6. Fill the TODOs in `src/main.ts` to add an event listener for the file form and a function to handle the form submit.
   - Note that the upload process is in two steps:
     1. Upload the file to the upload server. Take note of the returned file metadata.
     2. Add the file metadata to the GraphQL server
7. Test the upload. The added file should appear in the file list.

Commit all repos and push your changes to GitHub.
