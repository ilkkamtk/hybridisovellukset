# Using native device features in React Native (Expo SDK)

The [Expo SDK](https://docs.expo.dev/versions/latest/) is a collection of JavaScript APIs that give access to device and platform functionality like the camera, notifications, sensors, and much more. This means developers can easily integrate native features into their apps without dealing with native code.

## Lab assignment 1: Implement Upload screen with ImagePicker and React Hook Form

1. Continue the previous exercise. Merge it to _main_ branch and create a new development branch _upload_ with git for these tasks.
1. Create a new file _views/Upload.tsx_ for the upload functionality. Add function component and the usual imports and exports.
1. Add a new tab to bottom tab navigator for navigating to `Upload` component
1. Add input fields for `title` and `description` like in the `LoginForm.tsx`. Add two buttons for selecting and uploading a file. Add an `<Image>` component to show a preview of the selected image.
1. Study [ImagePicker](https://docs.expo.dev/versions/latest/sdk/imagepicker/) and use the example to select an image from `ImagePicker` and display it in `<Image>` when the first button is pressed.

    - When uploading a file to the API, you need to send [FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData/Using_FormData_Objects)
    - When appending file to FormData the object format is like this:

      ```ts
      formData.append('file', {
        uri: uri,
        name: originalFileName,
        type: mimeTypeOfFile,
      });
      ```

    - `uri:` get this from `ImagePicker` object that is stored in state
    - `originalFileName:` get this from `ImagePicker` object that is stored in state
    - `mimeTypeOfFile:` [MIME type/Media Type](https://en.wikipedia.org/wiki/Media_type) e.g. _image/jpeg_ or _video/mov_. You can get the first part from `type` of the `ImagePicker` object, the second part from `filename`

1. Send data with `onSubmit` function

   ```tsx
      // Upload.tsx
      ...
      const onSubmit = async (data) => {
         // TODO: if image is not selected, Alert error message and stop running this function
         // TODO: create new FormData object and append title and description
         // TODO: get filename
         // TODO: get extension
         // TODO: if extension is jpg change to jpeg because in MIME type it has to be jpeg. Hint: ternary operator
         // TODO: append image to FormData object
         // TODO: send FormData to API with postMedia() you made earlier
      }
      ...
   ```

1. Display an [`<ActivityIndicator>`](https://reactnative.dev/docs/activityindicator) when file is being uploaded or if you are using `<Button>` from React Native Elements, you can use [loading](https://reactnativeelements.com/docs/components/button#button-with-loading-spinner) prop. You'll need to add a new state (e.g. _loading_) to achieve this.
1. After the file is uploaded (promise state returned by the async upload function is _fulfilled_) redirect to _Home_ tab
    - there might be a need to wait 1-2 seconds before going to Home so that thumbnail (generated on the server) is ready. So use [setTimeout](https://developer.mozilla.org/en-US/docs/Web/API/setTimeout) or [Alert with confirmation](https://reactnative.dev/docs/alert)
1. After [navigating](https://reactnavigation.org/docs/navigation-actions/#navigate) to _Home_ tab the file list must be refreshed to see new files
    - add a new state _update_ to _Context_ to control the file list refreshing. Set default value to `false`.
    - send the `update` state as a parameter of `useMedia` in _Home.tsx_: `const {mediaArray} = useMedia(update);`
    - receive the parameter in `useMedia`: `const useMedia = (update) => {...`
    - modify `useEffect()` hook inside `useMedia()` hook to react when that state changes (hint: square brackets)
    - Now `loadMedia` is called every time the `update` state changes. Import and use `update` and `setUpdate` context states in _Upload.tsx_. When you have a successful upload, change the value of `update` to opposite (true/false) to trigger the update of `mediaArray`: `setUpdate(!update)`
1. Upload button should be activated only when the form is correctly filled and media file is selected
    - _Title_ is required and minimun length is ?
    - _Description_ minimun length is ?
    - Hint: Button component has a property `disabled`
1. Add reset button for clearing the whole form and image preview
    - add a new function `resetForm()` which resets all the things you want to reset
    - call `resetForm()` when reset button is pressed
    - also call `resetForm()` after file is uploaded or user navigates away from this screen with [useFocusEffect](https://reactnavigation.org/docs/use-focus-effect/)

### Extra

1. Show ActivityIndicators always when loading and uploading data
1. Could the code be cleaner? Maybe put some JSX into separate files (to components folder)
1. Dispay notifications to users when something goes wrong, upload fails, or similar

## Lab assignment 2: Fully featured Media Player

1. Continue the previous exercise. Create a new git branch `player` for these tasks.
1. Modify `Single.tsx`. Features:
    - Depending on file type use `<Image>` or `<Video>` to show/play media file
      - Video: <https://docs.expo.io/versions/latest/sdk/video/>
      - Audio is optional: <https://docs.expo.io/versions/latest/sdk/audio/>
    - Make sure you display also all other data related to the media file: title, description, owner, size, date, etc.
    - Include related components: likes, comments, tags, ratings, etc.
    - Optional: show users who like the image
1. Implement _modify_ and _delete_ buttons that are displayed only to the owner of the media file
1. Add corresponding functionality to the buttons
    - Implement `putMedia` and `deleteMedia` functions to `useMedia` hook in `apiHooks.ts`
    - Modify needs a new view with the same form as in the _Upload_ view
       - after deleting update the file list
    - Create `Modify.tsx` to `views` folder. Modify is 90% same as Upload
        - Start with making a copy of `Upload.tsx` and remove ImagePicker
        - Call `putMedia` instead of `postMedia` in submit function
    - Delete does not need it's own view, but a [confirmation dialog](https://reactnative.dev/docs/alert) might be useful before calling `deleteMedia`
    - Any case, do not bind delete function with params directly to the `onPress`, because it will be called immediately when the component is rendered. Use arrow function instead, example:

      ```tsx
      <Button onPress={() => {
        deleteFile(file.file_id);
      }}><Text>Delete</Text></Button>
      ```

    - not like this! because it's invoked immediately without click:

      ```tsx
      <Button onPress={ deleteFile(file.file_id) }>...
      ```

## Submit the assignment to Oma

1. Take screenshots of the app running on your phone/emulator
1. Place the screenshots to the project's folder `/screenshots`
1. Modify README.md: add the screenshots. e.g. `![Profile](screenshots/profile.png)`
1. Git add, commit & push to remote repository
1. Submit the link to correct branches of your repository to Oma, check that the link works and README.md is displaying the screenshots correctly

## Tips for the project (if using teacher's servers)

- You can add a tag with your app name automatically to all uploaded files. This way they don't get mixed with files uploaded by other apps.
- If you want to save additional data with files, you can add it to the 'description' field like this:

    ```javascript
      ...
      const moreData = {
        description: 'This is the actual description',
        someData: 'Some other data I want to save',
        someMoreData: {width: 300, height: 450 } 
      }
      ...
          // FormData
         formData.append('description', JSON.stringify(moreData));
      ...
      
    ```

    You can extract data later like this:

    ```javascript
    const allData = JSON.parse(descriptionFromAPI);
    const description = allData.description;
    const someData = allData.someData;
    const someMoreData = allData.someMoreData;
    ```
