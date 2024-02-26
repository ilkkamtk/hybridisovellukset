# Using native device features in React Native (Expo SDK)

The [Expo SDK](https://docs.expo.dev/versions/latest/) is a collection of JavaScript APIs that give access to device and platform functionality like the camera, notifications, sensors, and much more. This means developers can easily integrate native features into their apps without dealing with native code.

## Lab assignment 1: Implement File Upload Tab

### Step A - Upload screen with ImagePicker and React Hook Form

1. Continue the previous exercise. Merge it to _main_ branch and create a new development branch _upload_ with git for these tasks.
1. Create a new file `views/Upload.tsx` for the upload functionality. Add function component and the usual imports and exports.
1. Add a new tab to bottom tab navigator for navigating to `Upload` component
1. Add input fields for `title` and `description` like in the `LoginForm.tsx`.
1. Add an `<Image>` component to show a preview of the selected image or a placeholder image if no image chosen yet.
1. Add two buttons for selecting and uploading a file.
1. Study and install [ImagePicker](https://docs.expo.dev/versions/latest/sdk/imagepicker/) and use the example to select an image using `ImagePicker` and display it in `<Image>` when the first button (or placeholder image) is pressed. Add following functionality to the `Upload` component:

      ```ts
      ...
      // state for storing the image chosen with ImagePicker
      const [image, setImage] = useState<ImagePicker.ImagePickerResult | null>(null);
      ...
      // TODO: Use React Hook Form, set init values, etc.
      ...

      // This function is called when user wants to pick an image
      const pickImage = async () => {
        // No permissions request is necessary for launching the image library
        const result = await ImagePicker.launchImageLibraryAsync({
          mediaTypes: ImagePicker.MediaTypeOptions.All,
          quality: 0.6,
        });

        console.log(result);

        if (!result.canceled) {
          // TODO: Display the image in the <Image> component
        }
      };
      ...
      ```

    - `result.canceled`: when user cancels the image selection, this is `true`
    - `result.assets[0].uri`: the path to the image file in the device file system (use this to display the image in the `<Image>` component)
    - `result.assets[0].type:` type of the file (can be used for switching between image and video views)
    - `result.assets[0].mimeType:` file's [Media Type](https://en.wikipedia.org/wiki/Media_type) e.g. _image/jpeg_ or _video/mov_ (first part can be used for swithing between image and video views, second part for example for filtering specific image/video types)
    - `result.assets[0].fileName:` original filename of the image
1. For posting the file to the API, use [Expo FileSystem](https://docs.expo.dev/versions/latest/sdk/filesystem/) that provides access to a file system stored locally on the device and it's also capable of uploading and downloading files from network URLs
    - Install the package with `npx expo install expo-file-system`
    - Create a new function `postExpoFile` to `useFile` hook in `apiHooks.ts`:

      ```ts
      import * as FileSystem from 'expo-file-system';

      ...

      const postExpoFile = async (
        imageUri: string,
        token: string,
      ): Promise<UploadResponse> => {
        // TODO: display loading indicator
        const fileResult = await FileSystem.uploadAsync(
          process.env.EXPO_PUBLIC_UPLOAD_SERVER + '/upload',
          imageUri,
          {
            httpMethod: 'POST',
            uploadType: FileSystem.FileSystemUploadType.MULTIPART,
            fieldName: 'file',
            headers: {
              Authorization: 'Bearer ' + token,
            },
          },
        );
        // TODO: hide loading indicator
        return fileResult.body ? JSON.parse(fileResult.body) : null;
      };
      ...
      ```

1. Send data with `onSubmit` function

   ```tsx
      // Upload.tsx
      ...
      const onSubmit = async (inputs: {title: string; description: string}) => {
         // TODO: if image is not selected, Alert error message and stop running this function
         // TODO: read token
         // TODO: call postExpoFile() with image uri and token
         // TODO: get response and call postMedia() with the response data included
         // TODO: run context update (covered later)
         // TODO: reset the form and navigate to Home tab
      }
      ...
   ```

1. Display an [`<ActivityIndicator>`](https://reactnative.dev/docs/activityindicator) when file is being uploaded or if you are using `<Button>` from React Native Elements, you can use [loading](https://reactnativeelements.com/docs/components/button#button-with-loading-spinner) prop. You'll need to add a new state (e.g. _loading_) to your `useFile` hook to achieve this.
1. Upload button should be activated only when the form is correctly filled and media file is selected
    - _Title_ is required and minimun length is ?
    - _Description_ minimun length is ?
    - Hint: Button component has a property `disabled`
1. Add reset button for clearing the form and image preview
    - add a new function `resetForm()` which resets all the things you want to reset
    - call `resetForm()` when reset button is pressed
    - also call `resetForm()` after file is uploaded or user navigates away from this screen, so the form is cleared when user comes back to this screen, this can be done with `useEffect` hook and a cleanup function:

    ```ts
    useEffect(() => {
      const unsubscribe = navigation.addListener('blur', () => {
        resetForm();
      });
      return unsubscribe;
    }, []);
    ```

### Step B - Update the file list and navigate home after file upload

1. After the file is uploaded [navigate]((https://reactnavigation.org/docs/navigation-actions/#navigate)) automatically to _Home_ tab
    - You might want to [display a message](https://reactnative.dev/docs/alert) to the user that the file was uploaded successfully.
1. When navigating to _Home_ tab the file list must be refreshed to see new files
1. Create a new file `UpdateContext.ts` to `contexts` folder and add a new state _update_ to _Context_ to control the file list refreshing. Set default value to `false`:

    ```ts
    import {Dispatch, SetStateAction, createContext, useState} from 'react';

    type UpdateContextType = {
      update: boolean;
      setUpdate: Dispatch<SetStateAction<boolean>>;
    };

    const UpdateContext = createContext<UpdateContextType | null>(null);

    const UpdateProvider = ({children}: {children: React.ReactNode}) => {
      const [update, setUpdate] = useState<boolean>(false);

      return (
        <UpdateContext.Provider value={{update, setUpdate}}>
          {children}
        </UpdateContext.Provider>
      );
    };

    export {UpdateProvider, UpdateContext};
    ```

1. Create a new file `UpdateHook.ts` to `hooks` folder and add a new hook `useUpdateContext` for accessing the `update` state:

    ```ts
    import {useContext} from 'react';
    import {UpdateContext} from '../contexts/UpdateContext';

    // Current recommendation is to use custom hook instead of the context directly
    const useUpdateContext = () => {
      const context = useContext(UpdateContext);
      if (!context) {
        throw new Error('useUpdateContext must be used within an UpdateProvider');
      }
      return context;
    };

    export default useUpdateContext;
    ```

1. Provide the new context in `App.tsx`:

    ```tsx
    ...
    const App = () => {
      return (
        ...
        <UpdateProvider>
          <Navigator></Navigator>
          <StatusBar style="auto" />
        </UpdateProvider>
        ...
      );
    };
    ...
    ```

1. Modify `useEffect()` hook inside `useMedia()` hook to react when that state changes in context (hint: square brackets) to refresh the `mediaArray`.
    - Now `getMedia` is called every time the `update` state changes. Import and use `update` and `setUpdate` context state in `Upload.tsx`: `const {update, setUpdate} = useUpdateContext();`. When you have a successful upload, change the value of `update` to opposite (true/false) to trigger the update of `mediaArray`: `setUpdate(!update)`
1. You can use the new context and hook in the `Home` component to let the user refresh the list by swiping too, add also a loading indicator when the list is being refreshed:
    - Add a new state for loading indicator in `apiHooks.ts` / `useMedia` hook

    ```tsx
    const [loading, setLoading] = useState<boolean>(false);
    // TODO: Set true when starting to fetch data and false when data is fetched or fetch fails
    ```

    - Use the state and the new context in `Home.tsx` and add appropriate props to the `FlatList` component:

    ```tsx
      ...
      const {mediaArray, loading} = useMedia();
      const {update, setUpdate} = useUpdateContext();

      const onRefresh = () => {
        setUpdate(!update);
      };
      ...
      ...
        <FlatList
          data={mediaArray}
          renderItem={({item}) => <MediaListItem item={item} />}
          onRefresh={onRefresh}
          refreshing={loading}
        />
      ...
      ```

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

1. Implement the video file player support also on `Upload.tsx` view

## Submit the assignment to Oma

1. Take screenshots of the app running on your phone/emulator
1. Place the screenshots to the project's folder `/screenshots`
1. Modify `README.md`: add the screenshots. e.g. `![Profile](screenshots/profile.png)`
1. Git add, commit & push to remote repository
1. Submit the link to correct branches of your repository to Oma, _check that the link works and README.md is displaying the screenshots correctly_

## Tips for the project (if using teacher's servers)

- You can add a tag with your app name automatically to all uploaded files. This way they don't get mixed with files uploaded by other apps.
- If you want to save additional data with files, you can add it to the 'description' field like this:

    ```javascript
      const media = {
        title: 'My image',
        description: 'This is the actual description',
        ...
      };
      ...
      const moreData = {
        description: media.description,
        someData: 'Some other data I want to save',
        someMoreData: {width: 300, height: 450 } 
      };
      media.description = JSON.stringify(moreData));
      ...
    ```

    You can extract data later like this:

    ```javascript
    const allData = JSON.parse(descriptionFromAPI);
    const description = allData.description;
    const someData = allData.someData;
    const someMoreData = allData.someMoreData;
    ```
