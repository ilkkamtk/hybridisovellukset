# Shared State
## Context
- Context is a way to share state between components without having to pass props down the component tree manually at every level.
- Context is designed to share data that can be considered “global” for a tree of React components, such as the current authenticated user, theme, or preferred language.
- Context is primarily used when some data needs to be accessible by many components at different nesting levels.
- There are also other ways to share state between components, such as using Redux or Zustand if you need more advanced state management.

## Context API
- Context is created with `createContext`.
- It returns a `Provider` component and a `Consumer` component.
- **Provider:**
    - The `Provider` is used to wrap components and share the context values down the component tree. The `useContext` hook relies on the existence of a `Provider` higher up in the component tree to obtain the context values.

- **Consumer:**
    - Is a bit deprecated. It is used to consume the context values. It is not used as much as it used to be, because the `useContext` hook is more convenient.

Example:

**MyContext.js:**
```jsx
import React, { createContext, useContext } from 'react';

const MyContext = createContext();

export const MyProvider = ({ children }) => {
  const contextValue = "Hello from Context!";
  return (
    <MyContext.Provider value={contextValue}>
      {children}
    </MyContext.Provider>
  );
};

export { MyContext, MyProvider };
```

**App.js:**
```jsx
import React from 'react';
import { MyProvider } from './MyContext';
import ComponentA from './ComponentA';
import ComponentB from './ComponentB';

function App() {
  return (
    <MyProvider>
      <ComponentA />
      <ComponentB />
    </MyProvider>
  );
}

export default App;
```

**ComponentA.js:**
```jsx
import {useContext} from 'react';
import { MyContext } from './MyContext';

const ComponentA = () => {
  const contextValue = useContext(MyContext)
  return <div>Consumed via useMyContext in ComponentA: {contextValue}</div>;
};

export default ComponentA;
```

**ComponentB.js:**
```jsx
import {useContext} from 'react';
import { MyContext } from './MyContext';

const ComponentB = () => {
  const contextValue = useContext(MyContext)
  return <div>Consumed via useMyContext in ComponentB: {contextValue}</div>;
};

export default ComponentB;
```

## Sharing state with context
- Context is often used to share state between components.
- In the following example we will create a context for the currently logged-in user.
```tsx
// UserContext.tsx
import { createContext, useContext, useState } from 'react';
import { User } from '../types';

type UserContextType = {
  user: User | null;
  setUser: React.SetStateAction<User | null>;
};

const UserContext = createContext<UserContextType | null>(null);

export const UserProvider = ({ children }: { children: React.ReactNode }) => {
  const [user, setUser] = useState<User | null>(null);

  return (
    <UserContext.Provider value={{ user, setUser }}>
      {children}
    </UserContext.Provider>
  );
};

export { MainContext, MainProvider }
```
