# React Hooks

## Hook
The term hook often implies a mechanism for extending or customizing behavior by attaching or associating code with specific events or points in the program's execution.

## React Hooks
- React Hooks are functions that let us hook into the React state and lifecycle features from function components. 
- Hooks promote code reuse and organization by allowing logic to be extracted and shared across components.
- Hooks are a more direct way to use the React features such as state, lifecycle, context, and refs. E.g.:
    - `useState` for managing state
    - `useEffect` for handling side effects
      - A side effect refers to any external operation, such as data fetching or DOM manipulation, initiated by a component that occurs outside the usual component rendering process.
    - `useContext` for sharing state between components
- Hooks can be used in components or other hooks.
- Example of `useEffect` hook:
```tsx
import { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState<number>(0);

  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  }, [count]); // Only re-run the effect if count changes

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

## Rules of Hooks
- Hooks are JavaScript/TypeScript functions, but they impose two additional rules:
    - Only call Hooks at the top level of a component. Don’t call Hooks inside loops, conditions, or nested functions.
    - Only call Hooks from React components. Don’t call Hooks from regular JavaScript/TypeScript functions.
    - Hooks start with the word `use` so that React knows to treat them as Hooks.

## useState
- `useState` is a Hook that lets you add React state to function components.
  - state is a data structure that represents the parts of the app that can change.
  - The name "React" comes from the fact that React components are reactive, i.e., they react to state changes. So when you want to update the UI, you simply update the state, and React will automatically render the UI.
- `useState` returns a pair of values: the current state and a function that updates it.
- Example:
```tsx
import { useState } from 'react';

function Example() {
  const [name, setName] = useState<string>('');

    return (
        <div>
            <p>You entered: {name}</p>
            <input
                type="text"
                value={name}
                onChange={(e) => setName(e.target.value)}
            />
        </div>
    );
}
```

## useEffect
- `useEffect` is a Hook that lets you perform side effects in function components.
- Side effects are operations that affect other components or the outside world and can include data fetching, setting up a subscription, and manually changing the DOM.
- `useEffect` runs after the component renders and after every re-render.
- `useEffect` takes a function as an argument. This function is the effect.
- Second argument to `useEffect` is an array of values (dependencies). If any of the values change, the effect is re-run.
- Cleanup function can be returned from the effect. This function runs before the component is removed from the UI to prevent memory leaks.
- Example:
```tsx
import { useState, useEffect } from 'react';

function Example() {
  const [user, setUser] = useState<User | null>(null);

    useEffect(() => {
        fetchUser().then((user) => setUser(user));
        
        return () => {
            // Cleanup function
        };
    }, []);

    return (
        <div>
            {user ? (
                <div>
                    <p>Name: {user.name}</p>
                    <p>Email: {user.email}</p>
                </div>
            ) : (
                <p>Loading...</p>
            )}
        </div>
    );
}
```
