# Getting started with React Native Development

Read: [What is Hybrid Mobile App Development?](https://ionic.io/resources/articles/what-is-hybrid-app-development)

## React Native

React Native is an open-source framework developed by Facebook (now Meta) for building native mobile applications using JavaScript and React. It enables developers to use the same design principles as React for web development, but instead of targeting web browsers, React Native targets mobile platforms. Essentially, you can write your application's codebase in JavaScript and React, and React Native will compile it into native code for both iOS and Android platforms.

React Native is a versatile and efficient framework for developing mobile applications, offering the benefits of cross-platform development while maintaining a user experience that's close to native. While it is suitable for developing a wide range of applications, from simple utilities to complex, high-performance apps, it's essential to evaluate whether it's the right choice for a specific project. For applications requiring heavy computational work, intricate native animations, or extensive use of device-specific features, native development might be more appropriate.

Main advantages of React Native:

- Cross-Platform Development: One of the most significant advantages of React Native is its ability to create applications for both iOS and Android from a single codebase. This can lead to faster development times and easier maintenance compared to writing separate codebases for each platform.
- [Native Components](https://reactnative.dev/docs/intro-react-native-components): React Native uses native components under the hood. This means that the elements you use in your application compile to native UI components, which helps your application perform like a native app in terms of look, feel, and performance.
- [Hot Reloading](https://www.geeksforgeeks.org/difference-between-hot-reloading-and-live-reloading-in-react-native/): React Native supports hot reloading, which allows developers to immediately see the results of the latest changes without having to recompile the entire application. This feature enhances developer productivity by providing instant feedback.
- Developers who are familiar with JavaScript and React can easily transition to mobile development with React Native. The framework uses React's declarative UI paradigm and component-based structure, making it accessible to web developers.
- React Native has a large and active community, providing a wealth of libraries, tools, and frameworks that extend its capabilities. This ecosystem makes it easier to add features like navigation, gestures, and access to device hardware.

## Expo

[Expo](https://expo.dev/) is a toolchain built around React Native that simplifies the process of running, testing, and deploying applications. Expo provides a set of tools and services that can accelerate React Native development, especially for those new to mobile development or looking to prototype applications quickly.

### Main Tools and Features

- Simplified Setup: Expo offers a managed workflow that abstracts away much of the complexity involved in configuring the native development environment. Developers can start building a React Native app without having to install Android Studio or Xcode.
- [Expo CLI](https://docs.expo.dev/more/expo-cli/): A command-line tool that provides a set of commands for creating, developing, and building React Native applications.
- [Expo Go](https://expo.dev/go) App: Expo provides the Expo Go app, available on both iOS and Android, which allows developers and testers to open and preview React Native apps instantly on their devices without needing to build and compile the app natively.
- [Expo SDK](https://docs.expo.dev/versions/latest/): The Expo SDK is a collection of JavaScript APIs that give access to device and platform functionality like the camera, notifications, sensors, and much more. This means developers can easily integrate native features into their apps without dealing with native code.
- [Expo Snack](https://snack.expo.dev/): Expo offers an online tool called Snack, which lets you write React Native code in the browser and preview the results in real-time on a device or simulator.
- Over-the-Air Updates (OTA): Expo supports OTA updates, enabling developers to publish updates to their apps directly to users without going through the app stores' review processes each time.
- Easy Deployment and Building: With Expo, you can build your app's binary files (.apk for Android, .ipa for iOS) in the cloud, which simplifies the process of preparing your app for distribution through the Apple App Store and Google Play Store.

## Lab assignment 1: Creating a React Native Application with Expo

Study:

- [Environment setup](https://reactnative.dev/docs/environment-setup) and
- [Getting started and Learn the basics](https://facebook.github.io/react-native/docs/getting-started) from React Native documentation.
- [Using TypeScript with Expo](https://docs.expo.dev/guides/typescript/)

1. Install Expo app to your phone. [iOS](https://apps.apple.com/us/app/expo-client/id982107779) or [Android](https://play.google.com/store/apps/details?id=host.exp.exponent&hl=fi)
   - and/or install [Android Studio](https://developer.android.com/studio) and [set up an Android emulator](https://developer.android.com/studio/run/emulator).
   - and/or [install Xcode and set up an iOS simulator](https://developer.apple.com/documentation/safari-developer-tools/installing-xcode-and-simulators) (only available on macOS).
1. Use the [expo](https://docs.expo.dev/get-started/create-a-project) to generate an app skeleton
   - create a folder for your React Native projects
   - use Git Bash or terminal to go to this folder `cd foldername/otherfoldername/etc...`
   - `npx create-expo-app@latest --template`
     - choose 'Blank (TypeScript)' template
     - if this fails on Windows due to missing interactive mode, use power shell instead of Git Bash
   - or without "wizard": `npx create-expo-app@latest my-expo-app-name --template expo-template-blank-typescript`
1. [Configure ESlint and Prettier](https://docs.expo.dev/guides/using-eslint/) for your project:

   - `cd my-expo-app-name` & `npx expo lint`
   - Install prettier `npx expo install prettier eslint-config-prettier -- --save-dev` and create new file `.prettierrc.json` and add this content:

     ```json
     {
       "semi": true,
       "singleQuote": true,
       "tabWidth": 2,
       "useTabs": false,
       "bracketSpacing": false
     }
     ```

1. Create new file `.editorconfig` and add this content:

   ```conf
   root = true

   [*]
   indent_style = space
   indent_size = 2
   end_of_line = lf
   charset = utf-8
   trim_trailing_whitespace = true
   insert_final_newline = true
   ```

1. In VSCode fix potential curly-braces error in Preferences/Settings
   - search for 'braces' and uncheck all 'Insert space after...' checkboxes
1. Convert the App function to arrow function and add [react-native-safe-area-context](https://github.com/AppAndFlow/react-native-safe-area-context/blob/main/README.md):

    ```tsx
    import {StatusBar} from 'expo-status-bar';
    import {Platform, StyleSheet, Text} from 'react-native';
    import {SafeAreaProvider} from 'react-native-safe-area-context';

    const App = () => {
      return (
        <SafeAreaProvider style={styles.container}>
          <Text>Hello, React Native!</Text>
          <StatusBar style="auto" />
        </SafeAreaProvider>
      );
    };

    const styles = StyleSheet.create({
      container: {
        flex: 1,
        // quick fix for status bar in some Android devices
        paddingTop: Platform.OS === 'android' ? 30 : 0,
        backgroundColor: '#fff',
        alignItems: 'center',
        //justifyContent: 'center',
      },
    });

    export default App;
    ```

1. Test that the app works
   - Run `npm start`
   - Scan the QR code with your phone's camera (open link in Expo GO app). Your device and your computer need to be on the same network for this to work.
   - Alternatively, you can run the app on an emulator/simulator by pressing _a_ for Android or _i_ for iOS in the terminal.
   - Try debugger by adding some `console.log()` to `App.tsx` and press _j_ in the terminal to open the debugger.
   - Server can be stopped by pressing _ctrl-c_.
   - If having problems with the network connection between Expo GO and your development computer, try to use a network tunnel instead of direct LAN connection:
     - install ngrok: `npm install @expo/ngrok`
     - start the server with tunnel: `npm start -- --tunnel`
1. Create a new git branch `first`, add & commit you changes.
1. Create a new GitHub repository for your project and push the code to it.

## Lab assignment 2: Start Converting the existing React application to React Native

1. Create a new branch `native-components`
1. Copy `hooks/apiHooks.ts` and `types/LocalTypes.ts` files and `fetchData` function from the React project of the previous weeks into `src/` folder. Copy/install also the types from the [hybrid-types](https://github.com/ilkkamtk/hybrid-types) repo.
1. Copy the [environment variables](https://docs.expo.dev/guides/environment-variables/) (`.env.local` file) too and add it to `.gitignore`.
   - Replace the _VITE_ prefix with _EXPO_PUBLIC_ in the `.env.local` file.
   - Replace references like `import.meta.env.VITE_[VARNAME]` with `process.env.EXPO_PUBLIC_[VARNAME]` in the `apiHooks.ts` file.
   - NOTE: When using Expo Go on real mobile device, you need to have network access from your device to the servers (localhost is not valid server address anymore because the servers are not running on your mobile device).
   - Add also a sample file for `.env.local` to the repository.
1. Create `src/views/Home.tsx` and `src/components/MediaListItem.tsx` components.
   - They are very similar to their equivalents in 'plain' React app (`Home` and `MediaRow` components).
   - Use React Native [Core Components](https://reactnative.dev/docs/components-and-apis) like `<View>`, `<Text>`, `<Flatlist>` and `<Image>` instead of HTML elements.
   - Use `<TouchableOpacity>` instead of `<button>` to record taps.
   - Use [StyleSheet](https://reactnative.dev/docs/stylesheet) to style the components.
   - Convert `App.tsx` to use the new `Home` component.
1. Add, commit and push your branch to GitHub.
