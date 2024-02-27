# Next.js, getting started

## Installation

[Next.js docs](https://nextjs.org/docs/getting-started/installation)

### Assignment

**Create a new Next.js project called `my-next-app` and push it to GitHub.**

1. Run the following command in your terminal:

   ```bash
   npx create-next-app my-next-app
   ```

2. Answer the questions:

   ```text
   Would you like to use TypeScript? Yes
   Would you like to use ESLint? Yes
   Would you like to use Tailwind CSS? Yes
   Would you like to use `src/` directory? Yes
   Would you like to use App Router? (recommended) Yes
   Would you like to customize the default import alias (@/*)? No
   ```

   ```bash
   cd my-next-app
   npm run dev
   ```

3. Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

4. We are using the same types as earlier in this course. Copy them from previous project to the `src/types` folder of the
   new project or add the @sharedTypes path to tsconfig.json.

5. Check that the .gitignore file ignores your `node_modules` and IDE files like `.idea` and `.vscode`.

6. Create new repository in GitHub and push the project to GitHub.

---

## Next.js Project Structure

[Next.js docs](https://nextjs.org/docs/getting-started/project-structure)

### Assignment 1

#### Add pages to the project

1. Create two folders to the `src/app` folder: `profile` and `upload`:
   1. Add page.tsx to the `profile` folder
   2. Add page.tsx to the `upload` folder
2. Content for both pages:

   ```tsx
   const Page = () => {
     return (
       <main>
         <h1 className="text-4xl font-bold">
           Use one of these: Profile or Upload
         </h1>
       </main>
     );
   };

   export default Page;
   ```

3. Test the application in the browser. Open the following URLs:
   - [http://localhost:3000/profile](http://localhost:3000/users)
   - [http://localhost:3000/upload](http://localhost:3000/about)

---

## Routing

Routing in full-stack development refers to the process of directing network requests from a client to the appropriate
resources on a server or within a web application.

### Routing in Full-Stack Development

- **Path Navigation:** Routing manages how a web application responds to different URLs or paths requested by a client (
  usually a web browser).
- **Client-Side and Server-Side Routing:** In full-stack development, routing occurs both on the client side (in the
  browser) and the server side (on the server handling requests).

### Client-Side Routing

- **Client-Side Routing:** In client-side routing, the navigation and rendering of different views or components within
  a web application are handled by the browser. This often involves JavaScript frameworks like React, Angular, or
  Vue.js.
- **Changing Views without Full Page Reload:** Client-side routing enables seamless navigation within a web app by
  updating the URL and rendering content dynamically without reloading the entire page.

### Server-Side Routing

- **Server-Side Routing:** This refers to the process on the server that directs incoming requests to the appropriate
  resources or endpoints based on the requested URL.
- **Handling API Endpoints:** In server-side routing, requests to specific URLs or routes trigger different functions or
  controllers that process data, interact with databases, or perform other actions.

### Benefits and Importance

- **Organizing Application Logic:** Routing helps in organizing the application's structure by defining how different
  URLs correspond to various functionalities or views.
- **Separation of Concerns:** It promotes a separation of concerns, allowing developers to manage different parts of the
  application's functionality separately.

### Middleware and Routing

- **Middleware Integration:** Routing often involves the use of middleware, which intercepts requests, performs
  operations, or adds functionality before passing the request to the appropriate route handler.
- **Security and Authorization:** Middleware can be used in routing to enforce security measures, authentication, or
  authorization checks before allowing access to certain routes.

---

## Routing in Next.js

[Next.js docs](https://nextjs.org/docs/app/building-your-application/routing)

### Assignment 2

#### Add routing to the project

1. Modify `app/page.tsx`. Remove the default content from the return statement of the component and add the following
   content:

   ```tsx
   <main>
     <h1 className="text-4xl font-bold">Home</h1>
   </main>
   ```

2. Modify `app/layout.tsx`. Add a menu to navigate between pages:

   ```tsx
   <html lang="en">
     <body className={inter.className}>
       <nav className="flex items-center justify-between flex-wrap bg-teal-500 p-6">
         <div className="flex items-center flex-shrink-0 text-white mr-6">
           <span className="font-semibold text-xl tracking-tight">
             Example Site
           </span>
         </div>
         <div className="lg:flex lg:items-center lg:w-auto justify-end">
           <ul className="flex">
             <li className="mr-4">
               <a
                 href="/"
                 className="block mt-4 lg:inline-block lg:mt-0 text-teal-200 hover:text-white"
               >
                 Home
               </a>
             </li>
             <li className="mr-4">
               <a
                 href="/profile"
                 className="block mt-4 lg:inline-block lg:mt-0 text-teal-200 hover:text-white"
               >
                 Profile
               </a>
             </li>
             <li className="mr-4">
               <a
                 href="/upload"
                 className="block mt-4 lg:inline-block lg:mt-0 text-teal-200 hover:text-white"
               >
                 Upload
               </a>
             </li>
           </ul>
         </div>
       </nav>
       <div className="container mx-auto pt-4">{children}</div>
     </body>
   </html>
   ```

3. Test the application in the browser.

---

## Rendering

[Next.js docs](https://nextjs.org/docs/app/building-your-application/rendering)

---

## Server and Client components

### Server Components

- Server components are rendered on the server and sent to the client as HTML.
- Useful for performance, as they can be cached and reused.
- Not interactive and cannot be updated on the client.
- Useful for SEO, as search engines can crawl and index them easily.
- [Next.js docs](https://nextjs.org/docs/app/building-your-application/rendering/server-components)
- [Good video about React Server Components](https://www.youtube.com/watch?v=rGPpQdbDbwo)

### Client Components

- Client components are rendered on the client and can be updated dynamically.
- Useful for interactive components that need to be updated frequently.
- Not useful for SEO, as search engines cannot crawl and index them.
- [Next.js docs](https://nextjs.org/docs/app/building-your-application/rendering/client-components)

---

## Data Fetching

1. Fetch on the server or client
   - [Server](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating#fetching-data-on-the-server-with-fetch)
   - [Client](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
2. Third-party libraries on the server
   - For example database queries with [mysql2](https://www.npmjs.com/package/mysql2)
3. Route Handlers
   - For example [Dynamic Routes](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes)
4. Third-party libraries on the client
   - For example [SWR](https://swr.vercel.app/) or [React Query](https://react-query.tanstack.com/)

### Assignment 3

#### Get data from database and display it on the page

**NOTE:** You need to be connected to Metropolia network or VPN to access the database.

1. Install [mysql2](https://www.npmjs.com/package/mysql2) package:

   ```bash
   npm install mysql2
   ```

2. Create `.env.local` file to the root of the project and add the following content:

   ```text
   DB_HOST=mysql.metropolia.fi or localhost
   DB_USER=your_username
   DB_PASS=your_password
   DB_NAME=your_username
   UPLOAD_URL=http://localhost:3002/uploads/
   AUTH_API=http://localhost:3001/auth-api/api/v1
   UPLOAD_SERVER=http://localhost:3002/upload/api/v1
   JWT_SECRET=your_secret
   ```

3. Create `src/lib/db.ts` file and add the following content:

   ```ts
   import mysql from 'mysql2/promise';

   const promisePool = mysql.createPool({
     host: process.env.DB_HOST,
     user: process.env.DB_USER,
     password: process.env.DB_PASS,
     database: process.env.DB_NAME,
     waitForConnections: true,
     connectionLimit: 10,
     queueLimit: 0,
   });

   export { promisePool };
   ```

4. We are using the same database tables as earlier in this course. Copy all the model files to `src/models` folder of your project.
5. Also copy the contents of `src/lib` to `src/app/lib`.
6. Fix the paths in imports in the copied files: `../../src/lib` -> `@/app/lib/`
7. Create new file `src/components/MediaList.tsx` and add the following content:

   ```tsx
   import { fetchAllMedia } from '@/models/mediaModel';
   const MediaList = async () => {
     const mediaList = await fetchAllMedia();

     if (!mediaList) {
       return <p>No media found</p>;
     }

     return (
       <section className="flex flex-col p-8">
         <ul className="grid grid-cols-3 gap-4">
           {mediaList.map((item, index) => (
             <li
               key={index}
               className="flex flex-col items-center border border-gray-300 p-4 shadow-lg rounded-md bg-white"
             >
               <h3 className="text-lg font-bold self-start">{item.title}</h3>
               <p>Description: {item.description}</p>
               <p>
                 Date: {new Date(item.created_at).toLocaleDateString('fi-FI')}
               </p>
             </li>
           ))}
         </ul>
       </section>
     );
   };

   export default MediaList;
   ```

8. Modify `app/page.tsx` and add the following content:

   ```tsx
   import MediaList from '@/components/MediaList';

   const MediaPage = () => {
     return (
       <main>
         <h1 className="text-4xl font-bold">Media</h1>
         <MediaList />
       </main>
     );
   };

   export default MediaPage;
   ```

9. Test the application in the browser. Note that there are no images shown yet and that is OK.

---

## Authentication in Next.js

[App Router Authentication](https://www.youtube.com/watch?v=DJvM2lSPn6w)

### Cookies and Sessions

#### Cookies

Cookies are small pieces of data stored in the user's browser, often used for authentication, tracking, or personalization.

- **Client-Side Storage:** Cookies are stored in the user's browser and sent with each request to the server.

- **Expiration and Security:** Cookies can have an expiration date and can be secured with flags like `HttpOnly` and
  `Secure`.

- **Limitations:** Cookies have size and quantity limitations, and can be disabled by users.

- Example of setting a cookie:

  ```ts
  import { cookies } from 'next/headers';
  import { NextResponse } from 'next/server';

  const res = new NextResponse.next();
  res.cookies.set({
    name: 'user',
    value: 'John Doe',
    httpOnly: true,
    secure: true,
    expires: new Date('2022-01-01'),
  });
  ```

- Example of reading a cookie:

  ```ts
  import { cookies } from 'next/headers';

  export async function getCookie() {
    const cookie = cookies().get('user')?.value;
    console.log(cookie);
  }
  ```

#### Sessions

Sessions are server-side data storage mechanisms that maintain state for each user across multiple requests.

- **Server-Side Storage:** Session data is stored on the server, and a unique session identifier is sent to the client (often via a cookie).

- **Security and Flexibility:** Sessions can be more secure and flexible than cookies, and can store larger amounts of data since they are stored on the server.

- More on this in the next course. For now you can check out [Database Sessions](https://nextjs.org/docs/app/building-your-application/authentication#database-sessions).

#### Server Actions

"[Server Actions](https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations) are asynchronous functions that are executed on the server. They can be used in Server and Client Components to handle form submissions and data mutations in Next.js applications."

### Assignment 4

#### Add authentication to the project

1. Use the example from the video to add authentication to the project. The example files are in [this repository](https://github.com/balazsorban44/auth-poc-next/tree/main).
2. Create new file `src/app/lib/authActions.ts` and copy the content of `lib.ts` from the example repository.
3. Create new file `middleware.ts` to the root of your project and copy the content of `middleware.ts` from the example repository.
4. Create new file `src/app/login/page.tsx` and use this as the content (a modified version of the example from the video):

   ```tsx
   import { redirect } from 'next/navigation';
   import { getSession, login, logout } from '../lib/authActions';

   export default async function Page() {
     // const session = await getSession();

     return (
       <section>
         <div className="flex flex-col p-8">
           {/* {!session ? ( */}
           <form
             action={async (formData) => {
               'use server';
               await login(formData);
               redirect('/');
             }}
           >
             <div className="mb-4">
               <label
                 htmlFor="username"
                 className="block text-gray-700 text-sm font-bold mb-2"
               >
                 Username
               </label>
               <input
                 type="text"
                 name="username"
                 id="username"
                 placeholder="Username"
                 className="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
               />
             </div>
             <div className="mb-4">
               <label
                 htmlFor="password"
                 className="block text-gray-700 text-sm font-bold mb-2"
               >
                 Password
               </label>
               <input
                 type="password"
                 name="password"
                 id="password"
                 placeholder="Password"
                 className="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
               />
             </div>
             <button
               type="submit"
               className="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
             >
               Login
             </button>
           </form>
           {/* ) : ( */}
           <form
             action={async () => {
               'use server';
               await logout();
               // redirect('/');
             }}
           >
             <button
               type="submit"
               className="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
             >
               Logout
             </button>
           </form>
           {/* )} */}
         </div>
       </section>
     );
   }
   ```

5. Add link to the login page to the menu in `app/layout.tsx`.

6. Install `bcrypt` and `jsonwebtoken` packages:

   ```bash
   npm i bcrypt jsonwebtoken
   ```

7. In `src/app/lib/authActions.ts` we are going to use the user model instead of the Auth API. Copy the `userModel.ts` from the [Auth API repository](https://github.com/ilkkamtk/hybrid-auth-server/tree/main/src/api/models) to the `src/models` folder of your project.

8. In `src/app/lib/authActions.ts` delete the hard coded user and replace it with user from the database: `const user = await getUserByUsername(formData.get('username') as string);`

9. Check that you get user and the password is correct with `bcrypt.compareSync` method. Use [auhtCotroller.ts](https://github.com/ilkkamtk/hybrid-auth-server/blob/main/src/api/controllers/authController.ts#L29) as an example. Throw `new Error()` instead of `new CustomError()`.

10. Instead of `jose` use `jsonwebtoken` to create the token. Use the same JWT_SECRET as in the Auth API.

    - Delete the `encrypt()` and `decrypt()` functions and all references to them. Instead of `encrypt()` use `jwt.sign()` and instead of `decrypt()` use `jwt.verify()`.
    - Create the token the same way as in the [authController.ts](https://github.com/ilkkamtk/hybrid-auth-server/blob/main/src/api/controllers/authController.ts#L53).
    - Note that the video example's `session` variable is the same as `token` variable in the Auth API. Use `session` as the variable name.

11. Add [`expiresIn` property to the options](https://github.com/auth0/node-jsonwebtoken?tab=readme-ov-file#usage) in `jwt.sign` method. Use the `7d` as the value. This means that the token is valid for 7 days.

12. To set the expiration date to seven days in the future, you can use this as the value for the `expires` property in the `set` method of the `cookies` object: `new Date(Date.now() + 7 * 24 * 60 * 60 * 1000)`.

13. Modify `getSession()` and `updateSession()` functions to use jwt.verify() and jwt.sign() methods. The type for the `getSession()` function is `Promise<TokenContent | null>`.

14. To handle login errors, add a `error.tsx` to the `src/app/login` folder and use [this example](https://nextjs.org/docs/app/building-your-application/routing/error-handling#recovering-from-errors) as content.

15. To protect routes, add `requireAuth()` function to the `src/app/lib/authActions.ts`:

    ```ts
    export async function requireAuth() {
      const session = await getSession();
      if (!session?.user_id) {
        redirect('/');
      }
    }
    ```

    Then use this function in the `profile` and `upload` pages.

16. Add conditional rendering to the `app/layout.tsx` to show/hide `upload` and `profile` links based on the user's authentication status.

17. Test the application in the browser. Use the users that you have in your database from previous classes to test the login.

---

## Next.js API Endpoints

API endpoints allow you to handle various types of HTTP requests, enabling data retrieval, modification, or any
server-side logic needed for your application.

### Creating API Endpoints in Next.js

- **`api` Directory:** To define API routes, you can create a special directory named `api` within your `app` folder.
- **File-Based Routing:** Each file you create a folder with `route.ts` (or js) within the `api` directory
  automatically becomes an API endpoint, accessible at the corresponding URL path.

### Handling HTTP Requests

- **GET, POST, etc.:** API routes in Next.js can handle different types of HTTP requests such as GET, POST, PUT, DELETE,
  etc.
- **Route Handlers:** You define JavaScript functions within these files to handle these requests, accessing request
  parameters, headers, and body.

### Example of an API Endpoint

- For instance, creating a file named `app/api/media/route.ts` would create an API endpoint accessible at `/api/media`.
  You can define logic within this file to handle incoming requests, fetch data from a database, or perform any
  necessary operations.

### Integration with Frontend

- **Fetching Data:** Your frontend (React components or pages) can interact with these API endpoints using
  JavaScript's `fetch` or similar methods to perform CRUD (Create, Read, Update, Delete) operations.
- **Client-Side Usage:** These API endpoints can be used from client-side code to fetch or manipulate data without the
  need for a separate backend server.

### Middleware and Advanced Functionality

- **Middleware Support:** Next.js API endpoints can also utilize middleware for tasks like authentication,
  authorization, logging, or request validation.
- **Advanced Functionality:** They can be used to implement complex server-side logic, integrate with external services
  or databases etc.

### Assignment 5

#### Create a form and API endpoints to upload a media file

1. Create new file `app/components/MediaForm.tsx` and add the following content:

   ```tsx
   'use client';
   import { fetchData } from '@/utils/functions';
   import { useRouter } from 'next/navigation';

   const MediaForm = () => {
     const router = useRouter();

     const handleSubmit = async (e: React.FormEvent<HTMLFormElement>) => {
       e.preventDefault();

       try {
         // TODO: create form data and add the form content to it
         // TODO: send the form data to Next.js API endpoint /api/media using fetchData function
         // TODO: if result OK, redirect to the home page to see the uploaded media
       } catch (error) {
         console.error(error);
       }
     };
     return (
       <div className="flex flex-col p-8">
         <form onSubmit={handleSubmit}>
           <div className="mb-4">
             <label
               htmlFor="title"
               className="block text-gray-700 text-sm font-bold mb-2"
             >
               Title
             </label>
             <input
               type="text"
               name="title"
               id="title"
               className="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
             />
           </div>
           <div className="mb-4">
             <label
               htmlFor="description"
               className="block text-gray-700 text-sm font-bold mb-2"
             >
               Description
             </label>
             <input
               type="text"
               name="description"
               id="description"
               className="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
             />
           </div>
           <div className="mb-4">
             <label
               htmlFor="file"
               className="block text-gray-700 text-sm font-bold mb-2"
             >
               File
             </label>
             <input
               className="block w-full text-sm text-gray-900 border-2 border-gray-300 rounded-md shadow-sm cursor-pointer bg-gray-50 dark:text-gray-400 focus:outline-none dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 px-2 py-2"
               id="file_input"
               type="file"
               name="file"
             />
           </div>
           <button
             type="submit"
             className="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
           >
             Submit
           </button>
         </form>
       </div>
     );
   };

   export default MediaForm;
   ```

2. Modify `app/upload/page.tsx` and add `MediaForm` component to the page.

3. Create new file `app/api/media/route.ts` and add the following content:

   ```ts
   import { fetchData } from '@/app/lib/functions';
   import { postMedia } from '@/models/mediaModel';
   import { MediaItem } from '@sharedTypes/DBTypes';
   import { UploadResponse } from '@sharedTypes/MessageTypes';
   import { cookies } from 'next/headers';

   export async function POST(request: Request) {
     try {
       // TODO: get the form data from the request
       // TODO: get the token from the cookie
       // TODO: send the form data to the uppload server. See apiHooks from previous classes.
       // TODO: if the upload response is valid, add the media to the database
       // TODO: get title, description, size and type from the form data
       // TODO: get the filename from the upload response
       // TODO: create a media item object, see what postMedia funcion in mediaModel wants for input.
       // TODO: use the postMedia function from the mediaModel to add the media to the database. Since we are putting data to the database in the same app, we dont need to use a token.

       if (!postResult) {
         return new Response('Error adding media to database', { status: 500 });
       }
       result.message += ' ' + postResult.media_id;
       return new Response(JSON.stringify(result), {
         headers: { 'content-type': 'application/json' },
       });
     } catch (error) {
       console.error('Wtf' + (error as Error).message, error);
       return new Response((error as Error).message, { status: 500 });
     }
   }
   ```

4. Test the application in the browser.

Why do we need separate `MediaForm` client component instead if putting the form to the server component `upload/page.tsx`? Because the form is interactive; in this case that means the handleSumbit function. If a component is interactive and can be updated on the client, it has to be a client component. Server components can include client components, but not the other way around.

---

## What have we done so far?

1. Created a new Next.js project
2. Added routing to the project
3. Added a database connection to the project
4. Added data fetching to the project
5. Added authentication to the project
6. Added API endpoints to the project
7. Used external API to upload a file to the server

## Things to think about

1. Why `MediaList` component is a server component and `MediaForm` component is a client component?
   - MediaList component is a server component because it uses data straight from the database.
   - MediaForm component is a client component because file upload is handled by Next.js API endpoints and not by the component itself. Also if a component is interactive and can be updated on the client, it has to be a client component.
2. What are the benefits of using Next.js API endpoints instead of external API?
   - Same origin policy
   - Network latency
   - Type safety
   - Performance
   - Security
3. Why is file upload handled by external API and not by Next.js API endpoints?
   - Simplicity
   - Performance
   - Scalability
