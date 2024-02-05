# React Query (Tanstack Query)
[React Query](https://tanstack.com/query/latest) (now Tanstack Query) is a library for managing, caching, synchronizing and updating **server** state in React applications. It is built on top of React's built-in state management and hooks. 

"React Query is often described as the missing data-fetching library for React, but in reality, it's so much more than that. It's a comprehensive data management library that's designed to make it easy to fetch, cache, and update server state in your React applications." - Tanner Linsley, creator of React Query

"It works with any function that returns a Promise and embraces the stale-while-revalidate caching strategy*. The library operates on sane defaults that try to keep your data as fresh as possible while at the same time showing data to the user as early as possible, making it feel near instant at times and thus providing a great UX. On top of that, it is also very flexible and lets you customize various settings for when the defaults are not enough." - [TkDodo's blog](https://tkdodo.eu/blog/practical-react-query)

_* stale-while-revalidate caching strategy: When a request is made, the library returns the cached data immediately and then revalidates the data in the background. If the data has changed, the UI is updated with the new data._

## Why React Query?
- It is a comprehensive data management library that's designed to make it easy to fetch, cache, and update server state in your React applications.
- It works with any function that returns a Promise.

## Core concepts
### Queries
Queries are commonly used to fetch data from the server. They can be also used with any function that returns a Promise. React Query provides a `useQuery` hook to fetch data from the server. The `useQuery` hook takes a query key and a function that returns a Promise. The query key is used to identify the query and the function is used to fetch the data from the server.

## Client
The `QueryClient` is the core of React Query. It is used to manage the cache and the state of the queries. The `QueryClientProvider` is used to provide the `QueryClient` to the components in the application.

Example:
```tsx
import {
    QueryClient,
    QueryClientProvider,
    useQuery,
} from '@tanstack/react-query';
import {ReactQueryDevtools} from '@tanstack/react-query-devtools';

type Todo = {
    userId: number;
    id: number;
    title: string;
    completed: boolean;
};

const queryClient = new QueryClient();

export default function Example() {
    return (
        <QueryClientProvider client={queryClient}>
            <Content />
            <ReactQueryDevtools initialIsOpen />
        </QueryClientProvider>
    );
}

const fetchTodos = async (): Promise<Todo[]> => {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos');
    if (!response.ok) {
        throw new Error('Network response was not ok');
    }
    return response.json();
};

function Content() {
    const {isPending, error, data, isFetching} = useQuery({
        queryKey: ['todos'],
        queryFn: fetchTodos,
    });

    if (isPending) return 'Loading...';

    if (error) return 'An error has occurred: ' + error.message;

    return (
        <div>
            <div>{isFetching ? 'Updating...' : ''}</div>
            <h1>Todos</h1>
            <ul>{data && data.map((todo) => <li key={todo.id}>{todo.title}</li>)}</ul>
        </div>
    );
}
```

## React Query vs Context API
React Query is often compared to the Context API. The Context API is a built-in feature of React that is used to share data between components. It is often used to manage the state of the application. React Query is a library that is used to manage the state of the server. It is often used to fetch, cache, and update server state in React applications.
