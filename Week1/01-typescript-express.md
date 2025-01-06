# Express.js with TypeScript

### Why use TypeScript with Express.js?

- TypeScript can be used to define types for request and response objects in Express.js, making the code more readable and maintainable.
- TypeScript can be used to define interfaces for middleware functions, route handlers, and other components in an Express.js application.
- TypeScript can be used to define custom types for data validation, serialization, and other tasks in an Express.js application.

## Setting up a TypeScript project with Express.js

### Basic setup

1. Create a new directory for your project and navigate to it in the terminal.
2. Run `npm init -y` to create a new `package.json` file.
3. Install the required dependencies by running `npm i express` and `npm i -D typescript @types/node @types/express`.
4. Create a new `tsconfig.json` file in the root of your project with the following configuration:

    ```json
    {
      "compilerOptions": {
        "target": "ES6",
        "module": "commonjs",
        "outDir": "./dist",
        "rootDir": "./src",
        "strict": true,
        "esModuleInterop": true
      }
    }
    ```

5. Create a new `src` directory in the root of your project and add a new `index.ts` file with the following code:

    ```typescript
    import express, {Request, Response} from 'express';
    
    const app = express();
    const port = 3000;
    
    app.get('/', (req: Request, res: Response<string>) => {
      res.send('Hello World!');
    });
    
    app.listen(port, () => {
      console.log(`Server running on port ${port}`);
    });
    ```

6. Update the `package.json` file to include the following scripts:

    ```json
    {
      "scripts": {
        "start": "node dist/index.js",
        "dev": "nodemon ts-node src/index.ts",
        "build": "tsc"
      }
    }
    ```

7. Run `npm run dev` to start the development server with `nodemon` and `ts-node`.
8. Open your browser and navigate to `http://localhost:3000` to see the "Hello World!" message.

### Typing Request, Response, and NextFunction

Consider the following code:
```typescript
import {NextFunction, Request, Response} from 'express';

type Article = {
  id: number;
  title: string;
  content: string;
};

const articleGet = (req: Request<{id: string}>, res: Response<Article>, next: NextFunction) => {
    const article = getArticle(Number(req.params.id));
    res.json(article);
}; 
```
The `req` parameter is typed as `Request<{id: string}>`, which means that the `params` property of the `req` object is expected to have an `id` property of type `string`. The `res` parameter is typed as `Response<Article>`, which means that the `json` method of the `res` object expects an `Article` object as an argument. The `next` parameter is typed as `NextFunction`, which is the type for the next middleware function in the Express.js request-response cycle.

This way, TypeScript helps catch type errors at compile time and provides better code completion and documentation.

The `Request` and `Response` objects in Express.js can be typed using TypeScript's generics to specify the structure of the parameters, query, body, and headers in your route handlers and middleware functions.

- **`Request`** is a generic type provided by Express. It accepts **four type arguments**:
    1. `Params`: Defines the shape of the route parameters (e.g., `id` in `/users/:id`).
    2. `ResBody`: Defines the expected structure of the response body (though typically handled by the `Response` object).
    3. `ReqBody`: Defines the expected structure of the request body (e.g., JSON payloads).
    4. `ReqQuery`: Defines the expected structure of the query parameters (e.g., `/users?name=John`).

  If you omit any of these arguments, they default to `any`.

- **`Response`** is also a generic type, but it only takes **one type argument**:
    1. `ResBody`: Defines the structure of the response body being sent back to the client.
  2. `Locals`: Specifies the type of the locals object, which is a property of Response used to store data locally scoped to a request. This is particularly useful for sharing data between middleware and route handlers.

---

### **Example Usage:**

```typescript
import { Request, Response, NextFunction } from 'express';

// Define types for route parameters, request body, query, response body, and locals
type UserParams = {
    id: string;
};

type UserBody = {
    name: string;
    email: string;
};

type UserQuery = {
    active: boolean;
};

type UserResponse = {
    message: string;
    user?: {
        id: string;
        name: string;
        email: string;
    };
};

type Locals = {
    isAuthenticated: boolean;
};

// Middleware to set `locals`
const authenticationMiddleware = (
    req: Request,
    res: Response<any, Locals>,
    next: NextFunction
) => {
    // Set `isAuthenticated` in `locals`
    res.locals.isAuthenticated = true;
    next();
};

// Route handler
const getUserHandler = (
    req: Request<UserParams, {}, UserBody, UserQuery>,
    res: Response<UserResponse, Locals>
) => {
    const { id } = req.params; // `id` is typed as string
    const { active } = req.query; // `active` is typed as boolean
    const isAuthenticated = res.locals.isAuthenticated; // `isAuthenticated` is typed as boolean

    if (!isAuthenticated) {
        return res.status(401).json({ message: 'Unauthorized' });
    }

    res.json({
        message: `User with ID ${id} and active status ${active} retrieved.`,
        user: { id, name: 'John Doe', email: 'john.doe@example.com' },
    });
};

```

---

### **Key Points to Remember:**
1. **Default Values**: If you don't specify the generics for `Request` or `Response`, TypeScript will default their types to `any` which should be avoided for type safety. Instead, use `{}` or `unknown` to indicate that the type is not known or no need to specify like ResBody in `Request`.
2. **Flexibility**: You don't usually specify all type arguments. You can focus on just the ones relevant to your application (e.g., `Params` or `ResBody`).
3. **Separation of Concerns**: Use interfaces or type aliases to define reusable types for `Params`, `Body`, etc., to keep your code clean and maintainable.


## Error Handling in Express.js with TypeScript

### Basic Error Handling

In an Express.js application, you can handle errors by using middleware functions that have an additional `err` parameter. This parameter represents the error object passed to the `next` function. Here's an example of a basic error-handling middleware:

```typescript
import express, { Request, Response, NextFunction } from 'express';

const app = express();

app.get('/', (req: Request, res: Response) => {
  throw new Error('Something went wrong');
});

app.use((err: Error, req: Request, res: Response, next: NextFunction) => {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

The error handler middleware function should be defined after all other route handlers and middleware functions. It takes four parameters: `err`, `req`, `res`, and `next`. If an error occurs in any of the previous middleware functions or route handlers, the error will be passed to this error handler middleware.

### Custom Error Handling

You can create custom error classes to represent different types of errors in your application. Here's an example of a custom error class and how to use it in an Express.js application:

```typescript
// CustomError.ts
export default class CustomError extends Error {
  status = 400; // Default status code, can/should be overridden

  constructor(message: string, status: number) {
    super(message);
    this.status = status;
    Object.setPrototypeOf(this, CustomError.prototype);
  }
}

// app.ts
import express, { Request, Response, NextFunction } from 'express';

import CustomError from './CustomError';

const app = express();

app.get('/', (req: Request, res: Response, next: NextFunction) => {
   // do something that might cause an error
   if (errorCondition) {
     next(new CustomError('Custom error message', 400));
     return; // this is important to prevent the next middleware from running
   }
});

app.use((err: CustomError, req: Request, res: Response, next: NextFunction) => {
  console.error(err.stack);
  res.status(err.status).send(err.message);
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

In this example, the `CustomError` class extends the built-in `Error` class and adds a `status` property to represent the HTTP status code of the error. When an error occurs in a route handler, you can create an instance of `CustomError` and pass it to the `next` function to trigger the error handler middleware.

## Assignment

1. Clone the example server.
    - [express-ts-sqlite](https://github.com/ilkkamtk/express-ts-sqlite.git)
    - Go to the cloned directory `cd express-ts-sqlite`
    - Install dependencies and test run the server: 
       - `npm i`
       - `npm run dev`
    - Test the server by sending a GET request to `http://localhost:3000/api/v1/example`
2. Example uses [SQLite](https://www.sqlite.org/) as a database. SQLite is a serverless database that stores data in a single file. You can use it for development and testing purposes and also for production in small-scale applications.
   - The example server uses [better-sqlite3](https://www.npmjs.com/package/better-sqlite3) as the SQLite driver. Future versions of Node will have [built-in support for SQLite](https://blog.logrocket.com/using-built-in-sqlite-module-node-js/?ref=dailydev).
3. The example server has a simple REST API for managing articles. Your task is to add authors to the articles.
   - Create a new table for authors in the SQLite database. See `src/database/db.ts` for an example of how to create a table and insert initial data.
      - The table should have the following columns: `id`, `name`, `email`.
      - The `id` column should be an auto-incrementing primary key.
      - The `name` and `email` columns should be `TEXT` type.
      - Also create a TypeScript type for the author.
   - Modify the existing table 'articles' to include an 'author_id' column.
   - Add CRUD operations for authors.
      - Use existing model, controller and router files as a reference.
   - Associate authors with articles.
      - So when adding a new article, you can specify the author.
   - Modify the existing routes to handle authors.
      - Change the router `example` to `articles`. Add new router for authors.
