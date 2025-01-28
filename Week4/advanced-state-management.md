# Advanced state management in React

React's built-in state management is enough for many use cases. However, when the application grows, the state management can become complex. In React `useReducer` hook can be used to manage the state in a more organized way.

## useReducer

[`useReducer`](https://react.dev/reference/react/useReducer) is a hook that is used to manage state in a more organized way. It is similar to [Redux](https://redux.js.org/), which is a popular state management library for React. `useReducer` is a built-in hook in React, so you don't need to install any additional libraries to use it.

The term **reducer** comes from the [reducer function](https://medium.com/@jsmuster/understanding-reducers-d0f934aceccd) in functional programming. A reducer function is a function that takes an accumulator and a value and returns a new accumulator. In the context of `useReducer`, the reducer function takes the current state and an action and returns the new state.

[Dispatch function](https://react.dev/reference/react/useReducer#dispatch) is used to dispatch an action. An **action** is an object that describes what happened. It is a convention to use the `type` property to describe the action and the `payload` property to pass additional data.

`useReducer` is similar to `useState`, but it is more powerful. It is used when the state logic is complex and involves multiple sub-values or when the next state depends on the previous one.

Example:

```tsx
import {useReducer} from 'react';

type State = {
  count: number;
};

type Action = {
  type: 'increment' | 'decrement';
  payload?: number;
};

const initialState: State = {count: 0};

function reducer(state: State, action: Action): State {
  switch (action.type) {
    case 'increment':
      return {count: state.count + (action.payload ?? 1)};
    case 'decrement':
      return {count: state.count - (action.payload ?? 1)};
    default:
      throw new Error();
  }
}

export default function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
    </>
  );
}
```

In this example, we have a simple counter component. The state is an object with a `count` property. In the switch statement in the `reducer` function, we check the `action.type` and return the new state based on the action type. When the user clicks the `+` button, the `dispatch` function is called with the `increment` action. When the user clicks the `-` button, the `dispatch` function is called with the `decrement` action.

## Lab assignment 1

1. Create a new branch `likes` with git.
2. The goal of this exercise is to use `useReducer` instead of `useState` to handle a state with multiple sub-values. The state is used to show the number of likes and to show if the user has liked the media. The state should look like this:

   ```tsx
   type LikeState = {
     count: number;
     userLike: Like | null;
   };
   ```

   - In `LikeState` the `count` property is the number of likes and the `userLike` property is the user's like. It is `null` if the user hasn't liked the media.
3. Create a new component `Likes.tsx` to `components` folder.
4. Add a like button to the `Likes` component. The button should be shown only when the user is logged in. The button should be styled the same way as the `Show` button in `MediaRow`. Also add a paragraph to show the number of likes with a hard-coded number.
5. Add prop `item` to the `Likes` component. The `item` is the media item that the user can like. The type of the `item` is `MediaItemWithOwner` or `null` like in the `SingleView` component (because it is passed from the `SingleView` component).
6. Add `<Likes item={item} />` to the `SingleView` component where you want to show the like button and the number of likes.
7. To send and receive likes from the server, we need to add a new hook to `ApiHooks.ts`. The hook is called `useLike` and it is used to send and receive likes from the server. The hook should look like this:

   ```tsx
   const useLike = () => {
     const postLike = async (media_id: number, token: string) => {
       // TODO: Send a POST request to /likes with object { media_id } and the token in the Authorization header.
     };

     const deleteLike = async (like_id: number, token: string) => {
       // TODO: Send a DELETE request to /likes/:like_id with the token in the Authorization header.
     };

     const getCountByMediaId = async (media_id: number) => {
       // TODO: Send a GET request to /likes/count/:media_id to get the number of likes.
     };

     const getUserLike = async (media_id: number, token: string) => {
       // TODO: Send a GET request to /likes/bymedia/user/:media_id to get the user's like on the media.
     };

     return {postLike, deleteLike, getCountByMediaId, getUserLike};
   };
   ```

8. Add types for the state and the action after the imports in `Likes` component:

   ```tsx
   type LikeState = {
     count: number;
     userLike: Like | null;
   };

   type LikeAction = {
     type: 'setLikeCount' | 'like';
     like?: Like | null;
     count?: number;
   };
   ```

   - In `LikeAction` the `type` property is the action type. The `like` property is the like object and the `count` property is the new like count.

9. Add `likeInitialState` object after the `LikeAction` type:

   ```tsx
   const likeInitialState: LikeState = {
     count: 0,
     userLike: null,
   };
   ```

   - The initial state is used to set the initial state of the reducer when the component is first rendered.
10. Add the `likeReducer` function after the `likeInitialState` object:

    ```tsx
    function likeReducer(state: LikeState, action: LikeAction): LikeState {
      switch (action.type) {
        case 'setLikeCount':
          return {...state, count: action.count ?? 0};
        case 'like':
          if (action.like !== undefined) {
            return {...state, userLike: action.like};
          }
          return state; // no change if action.like is undefined
        default:
          return state; // Return the unchanged state if the action type is not recognized
      }
    }
    ```

    - Note the use of the spread operator (`...`) to copy the state and only change the properties that change.
    - Also note the use of the nullish coalescing operator (`??`) to provide a default value for the `count` property if `action.count` is `undefined` or `null`.
11. Add the `useReducer` hook to the `Likes` component:

    ```tsx
    const [likeState, likeDispatch] = useReducer(likeReducer, likeInitialState);
    ```

    - The `likeState` is the current state and the `likeDispatch` is the dispatch function.
12. Import the `useLike` hook to `Likes` component and all four functions from `ApiHooks.ts`:

    ```tsx
    const {postLike, deleteLike, getCountByMediaId, getUserLike} = useLike();
    ```

13. Create a function to get the user's like and like count:

    ```tsx
    // get user like
    const getLikes = async () => {
      const token = localStorage.getItem('token');
      if (!item || !token) {
        return;
      }
      try {
        const userLike = await getUserLike(item.media_id, token);
        likeDispatch({type: 'like', like: userLike});
      } catch (e) {
        likeDispatch({type: 'like', like: null});
        console.log('get user like error', (e as Error).message);
      }
    };

    // get like count
    const getLikeCount = async () => {
      // TODO: get like count and dispatch it to the state
    };
    ```

    - The `getLikes` uses the `getUserLike` function from the `useLike` hook to get the user's like and the `getCountByMediaId` function to get the like count and dispatches the results to the state.

14. Call the `getLikes` and `getLikeCount` function in the `useEffect` hook. It should be called when the component is first rendered and when the `item` changes.
15. Create a function `handleLike` to handle the like button click:

    ```tsx
    const handleLike = async () => {
      try {
        const token = localStorage.getItem('token');
        if (!item || !token) {
          return;
        }
        // If user has liked the media, delete the like. Otherwise, post the like.
        if (likeState.userLike) {
          // TODO: delete the like and dispatch the new like count to the state. Dispatching is already done in the getLikes and getLikeCount functions.
        } else {
          // TODO: post the like and dispatch the new like count to the state. Dispatching is already done in the getLikes and getLikeCount functions.
        }
      } catch (e) {
        console.log('like error', (e as Error).message);
      }
    };
    ```

16. Add the `handleLike` function to the like button's `onClick` event.
17. Use `likeState.count` to show the number of likes and `likeState.userLike` to conditionally render `like` or `unlike` to the like button.
18. Test the like button. You can use the `like` and `unlike` buttons in the `SingleView` component to test the like functionality.

## Submit

1. Run `npm build` or `npm run build`
2. Move build folder to your public_html
3. Test your app: `http://users.metropolia.fi/~username/likes`
4. Modify README.md. Change the link in `Open [X](X) to view it in the browser.` to point to the above link.
5. git add, commit & push to remote repository
6. Submit the link to correct branch of your repository to Oma
