# Component Libraries, Forms and Login

## Component Libraries

React Native component libraries are collections of pre-built, reusable components designed to speed up mobile development. Modern libraries are optimized for the **New Architecture (Fabric)**, providing high-performance UI elements that adhere to established design systems or offer deep customizability. Utilizing these libraries ensures consistency, improves accessibility, and leverages polished components that have been tested across various device types and screen sizes.

Recommended React Native Component Libraries include:

* **[NativeWind](https://www.nativewind.dev/):** The industry standard for styling React Native apps. It brings Tailwind CSS to the mobile ecosystem, enabling shared styling logic between web and mobile with zero-runtime performance overhead.
* **[React Native Reusables](https://rnr-docs.vercel.app/):** A collection of high-quality, copy-paste components built using NativeWind and accessible primitives. It provides a "shadcn/ui" experience for mobile, giving developers full ownership of their component code.
* **[Tamagui](https://tamagui.dev/):** A sophisticated UI kit and optimizing compiler. It is built for ultimate performance, automatically flattening component trees to native code to eliminate the rendering bottlenecks found in legacy libraries.
* **[gluestack-ui](https://gluestack.io/):** A modular, accessible UI library designed from the ground up for the New Architecture. It offers a utility-first approach that is highly compatible with modern developer workflows.
* **[React Native Paper](https://reactnativepaper.com/):** A robust implementation of Material Design. Maintained by industry leaders, it is a "batteries-included" choice for applications that require a stable, well-documented, and platform-agnostic design language.
* **[UI Kitten](https://akveo.github.io/react-native-ui-kitten/):** Based on the Eva Design System, this library is ideal for creating visually consistent apps with a focus on theming and design-led development.

### Benefits of Using Modern Component Libraries:

* **Native Performance:** Modern libraries are built to leverage the **Fabric renderer**, ensuring smooth animations and synchronous layout calculations.
* **Development Efficiency:** They provide complex components—such as Bottom Sheets, Modals, and Date Pickers—that are difficult to build from scratch while maintaining native-level feel.
* **Cross-Platform Consistency:** Many modern kits allow for "Write Once, Run Anywhere" styling, ensuring your app looks and behaves consistently on iOS, Android, and Web.
* **Accessibility (A11y):** High-quality libraries include built-in support for screen readers, haptics, and focus management, which are critical for professional-grade applications.

### Selection Criteria:

When selecting a library, prioritize **active maintenance** and **New Architecture compatibility**. Ensure the library is compatible with your current Expo SDK or React Native version. If a library relies on legacy native modules that have not been updated for current rendering engines, it may lead to performance issues or build failures in modern projects.

## Lab assignment 1: Convert the app we've made so far to use NativeWind and React Native Reusables

We use NativeWind and React Native Reusables in this lab assignment example. You are free to choose any other library in your individual project assignment.

1. Continue last exercise. Create a new branch 'comp-lib' with git.
1. Install [SafeAreaContext](https://docs.expo.dev/versions/latest/sdk/safe-area-context/): `npm install react-native-safe-area-context`
    - This is a dependency for React Native Elements and replaces the `SafeAreaView` component
    - Wrap the whole app with `<SafeAreaProvider>` in `App.tsx`
1. Install [React Native Reusables](reusables.md) (including NativeWind) to your project.
1. Modify your components to use appropriate components from React Native Reusables.
   - install RNR components with command line e.g. `npx @react-native-reusables/cli@latest add card` and use them in your components
   - remove all stylesheets, styles and unnecessary imports
   - Some components you may find useful for the task: `Button`, `Card`, `Input`...
   - You can add icons with [Expo Vector Icons](https://docs.expo.dev/guides/icons/)
1. For adding icons to bottom tabs refer this [example](https://reactnavigation.org/docs/customizing-tabbar/?config=dynamic#add-icons-for-each-tab)

## Lab assignment 2: Implementing login and registration functionality with React Hook Form

1. Continue last exercise. Create a new branch 'login' with git.
1. Study:
    - [React Hook Form](https://react-hook-form.com/get-started#ReactNative)
    - [React Native Forms using React-Hook-Form](https://www.akashmittal.com/react-native-forms-using-react-hook-form/)
1. Install React Hook Form `npm i react-hook-form`
1. Copy `contexts/UserContext.tsx` and `hooks/ContextHooks.ts` from the 'plain' React app to your React Native app
    - Remove all navigation related code not used with React Native: `useNavigate`, `useLocation` and function calls that use them
    - Replace all `localStorage` usage with [`AsyncStorage`](https://react-native-async-storage.github.io) (install with expo: `npx expo install @react-native-async-storage/async-storage`):

      ```ts
      import AsyncStorage from '@react-native-async-storage/async-storage';
      ...
      await AsyncStorage.setItem('token', result.token);
      ...
      const token = await AsyncStorage.getItem('token');
      ...
      await AsyncStorage.removeItem('token');
      ```

1. Wrap the `Navigator` with `UserProvider` in `App.tsx`
1. Create components `LoginForm.tsx` and `RegisterForm.tsx` to `components` folder and copy `views/Login.tsx` from the 'plain' React app to your React Native app
1. Add `Login` view to stack navigator in `Navigator.tsx` and display it instead of other navigation components if user is not logged in
   - check from `UserContext` if user is not null
   - after this you should be able to see the login view only
1. Implement `LoginForm` using React Hook Form:

    ```tsx
    const LoginForm = () => {
      const {handleLogin} = useUserContext();
      const initValues: Credentials = {username: '', password: ''};
      const {
        control,
        handleSubmit,
        formState: {errors},
      } = useForm({
        defaultValues: initValues,
      });

      const doLogin = async (inputs: Credentials) => {
        handleLogin(inputs);
      };

      return (
        <Card>
          <Controller
            control={control}
            rules={{
              required: {value: true, message: 'is required'},
            }}
            render={({field: {onChange, onBlur, value}}) => (
              <Input
                placeholder="Username"
                onBlur={onBlur}
                onChangeText={onChange}
                value={value}
                autoCapitalize="none"
                errorMessage={errors.username?.message}
              />
            )}
            name="username"
          />

          <Controller
            control={control}
            rules={{
              maxLength: 100,
              required: {value: true, message: 'is required'},
            }}
            render={({field: {onChange, onBlur, value}}) => (
              <Input
                placeholder="password"
                secureTextEntry
                onBlur={onBlur}
                onChangeText={onChange}
                value={value}
                errorMessage={errors.password?.message}
              />
            )}
            name="password"
          />
          <Button title="Login" onPress={handleSubmit(doLogin)} />
        </Card>
      );
    };
    ```

1. Add logout functionality to `Profile` view and `UserContext`:
    - Add a button to `Profile` view for logging out
    - Use `handleLogout` provided by `UserContext` and use it with the button
    - Use `AsyncStorage` to remove the token from the device
    - After the user state is updated in context, Navigator automatically displays the Login view
1. Implement `RegisterForm` similar to `LoginForm` but with additional fields for `email` and  password confirmation (`confirmPassword`)
    - Get api functions from `useUser` hook: `const {postUser, getUsernameAvailable, getEmailAvailable} = useUser();`
    - Username availability should be checked when the input field loses focus, implement your own validation function for this:

      ```ts
      rules={{
        // other rules here
        ...
        validate: async (value) => {
          try {
            const {available} = await getUsernameAvailable(value);
            console.log('username available?', value, available);
            return available ? available : 'Username taken';
          } catch (error) {
            console.log((error as Error).message);
          }
        },
      }}
      ```

    - Add proper validation rules for password
      - at least minimum length, but you might use more complex rules using a regex pattern, example:

        ```ts
        ...
        rules={{
          pattern: {
            // simple email regex pattern, do better? 
            value: /^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4}$/,
            message: 'not a valid email',
          },
          // other rules
        }}
        ...        
        ```

    - and email as well:
      - check availability
      - use proper regex pattern
    - Confirm password should match the actual password, you can do this by adding _validate_ to _rules_ of `<Controller>` of confirm password field
      - extract [_getValues_](https://react-hook-form.com/docs/useform/getvalues) from _useForm_ to get the value of password field
      - before sending the form to the API you'll need to delete confirm password property from the data object because API might reject data if it has extra properties.
1. In `Login` Add a link/button for switching between login/register forms ("No account yet?" or something).
1. Implement a token based auto login feature in `Login` component
    - Call `handleAutoLogin` from `UserContext` in `useEffect` **only once** when the component is rendered
1. Try the app on a real device. You can see that it's hard/impossible to write since keyboard is covering the input fields. Use [KeyboardAvoidingView](https://reactnative.dev/docs/keyboardavoidingview) (or ScrollView) in `Login.tsx` to fix the issue.
1. Hiding the keyboard has to be done manually:

   ```tsx
   <TouchableOpacity
        onPress={() => Keyboard.dismiss()}
        style={{flex: 1}}
        activeOpacity={1}
      >
    ...
    ...
   </TouchableOpacity>
   ```

## Lab assignment 3: Implement profile view and adapt the missing features from the 'plain' React app

1. Continue last exercise. Merge it to _main_ branch and create a new development branch _profile_ with git for these tasks.
1. Dislplay all user data in `Profile` view
   - Use `UserContext` to get user data
   - Use `Card` and `ListItem` components from React Native Elements to display user data
   - Extra: Add user profile update functionality to profile page (similar to registration form)
1. Add new view `MyFiles.tsx` and add it to stack navigator
1. Add a button on Profile page which opens MyFiles:
   - Display a list of user's own files, very similar to Home: You can use Home and MediaListItem as example and make new components
1. Refer to the 'plain' React app and add likes, comments, tags and ratings to the `Single` view
   - For ratings, you can use [react-native-star-rating-widget](https://github.com/bviebahn/react-native-star-rating-widget)
   - Implementing upload page with image picker, and other native apis will be covered in the next lab assignments
1. Add and commit changes to git, push to Github.

---

## Tips'n tricks for styling and animations

- [Adding SVG support](https://kumar2396jayant.medium.com/how-to-use-svg-in-react-native-e581eca59534)
- [Lottie](https://airbnb.design/lottie/)
  - [Create Lottie without After Effects](https://www.youtube.com/watch?v=zoBMb72UDeI)
      1. [MacOS download](https://github.com/HaikuTeam/animator/releases/)
      2. Build with Windows
        1. [Step 1](https://github.com/HaikuTeam/animator#windows-os-dependencies) (no need for git or node installation, you have them already)
        2. [Step 2](https://github.com/HaikuTeam/animator#2-install-project-dependencies)
        3. [Step 3](https://github.com/HaikuTeam/animator#3-start-development-server)
  - [Convert SVG to Lottie](https://lottiefiles.com/svg-to-lottie/convert)
- [Masonry list](https://github.com/hyochan/react-native-masonry-list#react-native-masonry-list)
