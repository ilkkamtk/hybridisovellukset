# Zustand
Zustand is a library for managing state in React applications. It is similar to Redux, but with a much simpler API and less boilerplate. It is also built with hooks, so you can use it in functional components without needing to use classes.

Zustand is simple and unopinionated. It doesn't wrap the applications in a provider component, so you can use it in any component you like. You can use it with any other library you like.

Example:

```tsx
// store.ts
import create from 'zustand';

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
