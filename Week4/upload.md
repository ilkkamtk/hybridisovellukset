# File Upload in React

## [FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData)
- FormData is a built-in object in the browser that allows us to create key-value pairs of data, which can then be sent to
  the server in a POST request.
- Uses the same format as a form with `enctype="multipart/form-data"`

## Lab assignment 1
1. Continue last exercise. Create a new branch 'upload' with git.
2. Create `Upload.tsx` to `views`
3. Add `upload` to routing in App.tsx
    * Add `Upload` to imports
    * Add `<Route path="/upload" element={<Upload/>}/>` to `Routes`
    * Add `<Link to="/upload">Upload</Link>` to `Nav` in `Layout.tsx`
4. Add `<form>` to Upload.tsx with file input and submit button:
    ```tsx
    // Upload.tsx
    const Upload = () => {
        const [file, setFile] = useState<File | null>(null);
        return (
            <>
                <h1>Upload</h1>
                <form onSubmit={handleSubmit}>
                    <div>
                        <label htmlFor="title">Title</label>
                        <input
                            name="title"
                            type="text"
                            id="title"
                            onChange={handleInputChange}
                        />
                    </div>
                    <div>
                        <label htmlFor="description">Description</label>
                        <textarea
                            name="description"
                            rows={5}
                            id="description"
                            onChange={handleInputChange}
                        ></textarea>
                    </div>
                    <div>
                        <label htmlFor="file">File</label>
                        <input
                            name="file"
                            type="file"
                            id="file"
                            accept="image/*, video/*"
                        onChange={handleFileChange}
                        />
                    </div>
                    <img
                        src={
                            file
                            ? URL.createObjectURL(file)
                            : 'https://via.placeholder.com/200?text=Choose+image'
                        }
                        alt="preview"
                        width="200"
                    />
                    <button
                        type="submit"
                        disabled={file && inputs.title.length > 3 ? false : true}
                    >
                        Upload
                    </button>
                </form>
            </>
        );
    };
    ```
5. We can use `useForm` hook to handle `title` and `description` but for file input we need to create a new function `handleFileChange`:
    ```tsx
    // Upload.tsx
    const handleFileChange = (e: React.ChangeEvent<HTMLInputElement>) => {
        if (e.target.files) {
            console.log(e.target.files[0]);
        }
    };
    ```
6. Submitting the form:
    ```tsx
    // Upload.tsx
    const doUpload = async () => {
        try {
            // TODO: call postFile function (see below)
            // TODO: call postMedia function (see below)
            // TODO: redirect to Home
        } catch (e) {
            console.log((e as Error).message);
        }
    };
    ```
   - To upload the file to the file server, create a new hook `useFile` to `ApiHooks.ts`. Then add a new function `postFile`to `useFile` hook:
    ```tsx
    // ApiHooks.ts
    const postFile = async (file: File, token: string) => {
        // TODO: create FormData object
        // TODO: add file to FormData
        // TODO: upload the file to file server and get the file data
        // TODO: return the file data. The type is UploadResponse
    };
    ```
   - Then add new function `postMedia` to `useMedia` hook:
    ```tsx
    // ApiHooks.ts
    const postMedia = async (file: UploadResponse, inputs: Record<string, string>,
    token: string) => {
        // TODO: create a suitable object for Media API, the type is MediaItem without media_id, user_id, thumbnail and created_at. All those are generated by the API. Remember to add app_id from .env.local
        // TODO: post the data to Media API and get the data as MediaResponse
        // TODO: return the data
    };
    ```
    - Finally, add `PostFile` and `postMedia` to `doUpload` function.
    - Test the app. Upload a file and check that it is added to the media list.
   

## Submit
1. Run `npm build` or `npm run build`
2. Move build folder to your public_html
3. Test your app: `http://users.metropolia.fi/~username/upload`
4. Modify README.md. Change the link in `Open [X](X) to view it in the browser.` to point to the above link.
5. git add, commit & push to remote repository
6. Submit the link to correct branch of your repository to Oma
   