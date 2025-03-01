# React Native Routing and Navigation

- [React Navigation](https://reactnavigation.org/docs/getting-started/)
- [Stack Navigation](https://reactnavigation.org/docs/hello-react-navigation/) and [createStackNavigator](https://reactnavigation.org/docs/stack-navigator)
- [Tab Navigation](https://reactnavigation.org/docs/bottom-tab-navigator)

## Lab assignment 1

1. Create a new React Native project called 'Stack' with Expo CLI. Make this separate from the app we did in previous labs. No need to submit this or push it to Git
1. Follow [Hello React Navigation](https://reactnavigation.org/docs/hello-react-navigation/) and [Moving between screens](https://reactnavigation.org/docs/navigating/) articles to create a simple stack navigation

## Lab assignment 2

1. Create a new react native project called 'Tabs' with Expo CLI. Make this separate from the app we did in previous labs. No need to submit this or push it to Git
1. Follow [Tab Navigation](https://reactnavigation.org/docs/bottom-tab-navigator) article to create a simple tab navigation

## Lab assignment 3

### Step A - Tab navigation: navigate between 'Home' and 'Profile' views

1. Continue the app made in previous labs (**not** Labs 1 and 2 from this page).
   1. Make sure the previous branch is committed, then merge it with `main` branch.
      - (`git commit... etc`)
      - (`git checkout main`)
      - (`git merge branch-name`)
   1. Then create a new branch `navigation` from the `main` branch. (`git checkout -b navigation`).
   1. Creating branches from the `main` makes more sense than creating branches from other branches so use this method every time when instructions tell you to create a new branch.
   1. Submit still links to branches when submitting assignments to Oma
1. Goal is to make a navigation between three 'pages' usually called 'views' or 'screens' in React Native
   - Bottom tab menu has two links: 'Home' and 'Profile'
   - Each thumbnail is TouchableOpacity and tapping them should take to 'Single' to show the selected media file (just images at this point)
1. Install react-navigation with npm `npm i @react-navigation/native`
1. Install react-navigation-bottom-tabs with npm `npm i @react-navigation/bottom-tabs`
1. Install react-navigation-stack with npm `npm i @react-navigation/native-stack`
1. Install these packages with expo to get correct versions: `npx expo install react-native-screens react-native-safe-area-context`
1. Add `Single` and `Profile` components to `/views`, add first just some simple `Text` components to show that the navigation works
    - `Home` will be the component that should show first when the app starts
    - `Single` will be the component that should show when a thumbnail is tapped
    - `Profile` will be the component that should show when the 'Profile' tab is tapped
1. Create tab navigation
   - create new folder and file `navigators/Navigator.tsx`
   - in `Navigator.tsx` use `createBottomTabNavigator` to make a simple tab navigation to switch between _Home_ and _Profile_ views

   ```tsx
   import {createBottomTabNavigator} from '@react-navigation/bottom-tabs';
   import {NavigationContainer} from '@react-navigation/native';

   const Tab = createBottomTabNavigator();

   const Navigator = () => {
     return (
       <NavigationContainer>
         {/* TODO: Make tab navigator here */}
       </NavigationContainer>
     );
   };

   export default Navigator;
   ```

   - Modify `App.tsx` (remember to add neccessary and remove unneccessary imports. If you still need safe area view and styles, you need to move them to view components):

   ```tsx
   const App = () => {
     return (
       <>
         <Navigator></Navigator>
         <StatusBar style="auto" />
       </>
     );
   };
   ```

   - The app should at this point have a tab navigation between _Home_ and _Profile_ views

### Step B - Stack navigation: navigate to 'Single' component by tapping thumbnails

1. For this we need to [nest navigators](https://reactnavigation.org/docs/nesting-navigators) in `Navigator.tsx`:

   ```tsx
   // TODO: add neccessary imports

   // add after createBottomTabNavigator
   const Stack = createNativeStackNavigator();

   const TabScreen = () => {
     return (
       // TODO: move content of <NavigationContainer> here
     );
   };

   const StackScreen = () => {
     return (
       <Stack.Navigator>
         // TODO: make two stack screens:
         // 1st: name=Tabs, component=TabScreen, options=hide header
         // 2nd: name=Single, component=Single
       </Stack.Navigator>
     );
   };

   const Navigator = () => {
     return (
       <NavigationContainer>
         <StackScreen/>
       </NavigationContainer>
     );
   };

   export default Navigator;
   ```

   1. Pass `navigation` prop from `Home` to `MediaListItem` and use `navigate`-method to navigate to `Single`-component:

   ```tsx
   // Home.tsx
   const Home = ({navigation}: {navigation: NavigationProp<ParamListBase>}) => {
     ...
     return (
       <FlatList
         ...
         renderItem={
           ({item}) => <ListItem
             navigation={navigation}
             item={item}
           />
         }
         ...
       />
     );
   };

   // MediaListItem.tsx
   <TouchableOpacity
         ...
         onPress={
           () => {
             navigation.navigate('Single');
           }
         }
         ...
       >
   ```

    - Alternatively, you can [use the `useNavigation` hook to access the `navigation` prop](https://reactnavigation.org/docs/use-navigation/).  Here is an example how to [use it with TypeScript.](https://gist.github.com/ilkkamtk/1ee2cc6cd649e1ace5447b8e26815e46)
1. The app should at this point navigate to `Single` component when any of the thumbnails is tapped

### Step C - Show selected file in 'Single' component

1. Study [Passing parameters to routes](https://reactnavigation.org/docs/params/)
1. In `MediaListItem` you have file data in prop (`item`). Pass the file data as a parameter with `navigation.navigate`
1. In `Single.tsx` receive the file parameters and use it's properties to display the image files with `<Image>` component
1. Video files can played with Expo SDK's [`<Video>` component in expo-av](https://docs.expo.dev/versions/latest/sdk/av/) or using [expo-video](https://docs.expo.dev/versions/latest/sdk/video/)
1. Display file info using `<Text>` components
1. Git add, commit & push your _navigation_ branch to the remote repository and merge it to the _main_ branch

### Step D - Asynchronous image loading (Optional)

1. Study [Asynchronous image loading](https://snack.expo.io/HkjHS1ttZ)
1. Use `<AsyncImage>` instead of `<Image>` in the whole app to show text 'Loading' or [ActivityIndicator](https://docs.expo.io/versions/latest/react-native/activityindicator/) when images are loading
1. Git add, commit & push your _navigation_ branch to the remote repository and merge it to the _main_ branch
