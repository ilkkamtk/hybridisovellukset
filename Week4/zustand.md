# Zustand
Zustand is a library for managing state in React applications. It is similar to Redux, but with a much simpler API and less boilerplate. It is a small library that is easy to learn and use. It is a good choice for small to medium-sized applications.

Zustand is simple and unopinionated. It doesn't wrap the applications in a provider component, so you can use it in any component you like. You can use it with any other library you like.

Example:

```tsx
// store.ts
import {create} from 'zustand';

type State = {
  count: number;
  increment: () => void;
  decrement: () => void;
};

export const useStore = create<State>((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
}));
```
Usage:

```tsx
import { useStore } from './store';

export default function Counter() {
  const { count, increment, decrement } = useStore();

  return (
    <>
      Count: {count}
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </>
  );
}
```

In this example, we have a simple counter component. The `useStore` hook is used to get the `count`, `increment`, and `decrement` from the store. The `increment` and `decrement` functions are used to update the `count` in the store. When the user clicks the `+` button, the `increment` function is called. When the user clicks the `-` button, the `decrement` function is called.


## Lab assignment 1
1. Create a new branch `zustand` with git.
2. The goal of this exercise is to add commenting to the media items. The comments are shown in the `SingleView` component. The comments are shown always, but the user can add a new comment only when the user is logged in. The state of the comments is managed with Zustand.
3. Install Zustand to your project:
   ```bash
   npm install zustand
   ```
4. Create a new file `store.ts` to the `src` folder.
5. Add the following code to the `store.ts` file:
   ```tsx
   type CommentStore = {
      comments: Partial<Comment & {username: string}>[];
      addComment: (comment: Partial<Comment & {username: string}>) => void;
   };
   
   export const useCommentStore = create<CommentStore>((set) => ({
      comments: [],
      addComment: (comment) =>
              set((state) => ({
                 comments: [
                    ...state.comments,
                    {
                       comment_id: state.comments.length + 1, // This is a temporary solution
                       comment_text: comment.comment_text,
                       user_id: comment.user_id,
                       media_id: comment.media_id,
                       created_at: new Date(),
                       username: comment.username,
                    },
                 ],
              })),
   }));
   ```
   - In this example, we have a simple store for the comments. The `useStore` hook is used to get the `comments` and `addComment` from the store. The `addComment` function is used to add a new comment to the store. Note that the comments are not yet fetched/sent to the server. 
6. Create a new component `Comments.tsx` to the `components` folder. Add the component to the `SingleView` component where you want to show the comments.
7. Add a form to the `Comments` component to add a new comment. 
    - The form should be shown only when the user is logged in. 
    - The form should have an input field for the comment and a submit button.
    - Use the previously made `formHooks` to handle the form. See `LoginForm` for an example.
    - When submitting the form, the `addComment` function from the store should be called with the comment_text, media_id, and user_id.
    - Use `useRef` to reset the form after the form is submitted.
8. Add the comments to the `Comments` component. Use the `map` function to map the comments to `li` elements after the form.
9. Test that the comments are shown and that the form works.

## Lab assignment 2
1. Continue in the same branch.
2. Now we post the comments to the api. We need to add a new hook to `apiHooks.ts`. The hook is called `useComment` and it is used to send and receive comments from the server. The hook should look like this:
   ```tsx
   const useComment = () => {
      const postComment = async (
         comment_text: string,
         media_id: number,
         user_id: number,
         token: string) => {
          // TODO: Send a POST request to /comments with the comment object and the token in the Authorization header.
      };
   
      const getCommentsByMediaId = async (media_id: number) => {
          // TODO: Send a GET request to /comments/bymedia/:media_id to get the comments.
          // TODO: Send a GET request to auth api and add username to all comments
      };
   
      return { postComment, getComments };
    };
    ```
3. Modify `store.ts`. Add a new function `setComments` to the store. The `setComments` function is used to set the comments to the store after fetching them from the server. The `setComments` function should look like this:
   ```tsx
   export const useCommentStore = create<CommentStore>((set) => ({
      comments: [],
      setComments: (comments) =>
              set(() => ({
                 comments: comments,
              })),
      addComment: (comment) =>
              set((state) => ({
                 comments: [
                    ...state.comments,
                    {
                       comment_id: state.comments.length + 1, // This is a temporary solution
                       comment_text: comment.comment_text,
                       user_id: comment.user_id,
                       media_id: comment.media_id,
                       created_at: new Date(),
                       username: comment.username,
                    },
                 ],
              })),
      
   }));
   ```
   - Add `setComments: (comments: Partial<Comment & {username: string}>[]) => void;` to the `CommentStore` type.
4. Modify the `Comments` component. Add a `useEffect` hook to fetch the comments from the server when the component is mounted. Use the `getCommentsByMediaId` function from the `useComment` hook to fetch the comments. Use the `setComments` function from the store to set the comments to the store.
5. Test that the comments are fetched from the server and that the form works.
   - New comments are not saved to the server yet.
6. Modify the `Comments` component to post the comments to the server. Use the `postComment` function from the `useComment` hook to post the comments to the server. Use the `addComment` function from the store to add the comments to the store. 
7. Test the app and add a few comments with different users. Note that even if the new comment is saved to the API it is still shown from the local state. The comments should be fetched from the server after the comment is posted to the server. This is done to get the comment_id and created_at values from the server. We don't have to use `addComment` at all: Call `getCommentsByMediaId` after `postComment` and set the comments to the store with `setComments`.
8. Test that the comments are fetched from the server and that the form works.

## Submit
1. Run `npm build` or `npm run build`
2. Move build folder to your public_html
3. Test your app: `http://users.metropolia.fi/~username/zustand`
4. Modify README.md. Change the link in `Open [X](X) to view it in the browser.` to point to the above link.
5. git add, commit & push to remote repository
6. Submit the link to correct branch of your repository to Oma
