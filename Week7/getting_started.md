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

4. Download this [ZIP file]() and extract it. It contains a folder called `Types`. Copy it to the `src/types` folder of
   the project. This folder contains TypeScript types for the project.

5. Check that the .gitignore file ignore your node_modules and IDE files.

6. Create new repository in GitHub and push the project to GitHub.

---

## Next.js Project Structure

[Next.js docs](https://nextjs.org/docs/getting-started/project-structure)

### Assignment

**Add pages to the project**

1. Create three folders to the `src/app` folder: `media`, `users` and `about`:
    1. Add page.tsx to the `media` folder
    2. Add page.tsx to the `users` folder
    3. Add page.tsx to the `about` folder
2. Content for all pages:

    ```tsx
    const Page = () => {
        return (
            <main>
                <h1 className="text-4xl font-bold">Use one of these: Media, Users or About</h1>
            </main>
        );
    };
    
    export default Page;
    ```
3. Test the application in the browser. Open the following URLs:
    - [http://localhost:3000/media](http://localhost:3000/media)
    - [http://localhost:3000/users](http://localhost:3000/users)
    - [http://localhost:3000/about](http://localhost:3000/about)

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

Routing in full-stack development is a fundamental aspect that determines how users navigate through web applications,
how requests are handled, and how different parts of an application's functionality are organized and accessed.

---

## Routing in Next.js

[Next.js docs](https://nextjs.org/docs/app/building-your-application/routing)

### Assignment

**Add routing to the project**

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
          <div className="w-full block flex-grow lg:flex lg:items-center lg:w-auto">
            <div className="lg:flex-grow"></div>
            <div>
              <a
                href="/"
                className="block mt-4 lg:inline-block lg:mt-0 text-teal-200 hover:text-white mr-4"
              >
                Home
              </a>
              <a
                href="/media"
                className="block mt-4 lg:inline-block lg:mt-0 text-teal-200 hover:text-white mr-4"
              >
                Media
              </a>
                <a
                    href="/users"
                    className="block mt-4 lg:inline-block lg:mt-0 text-teal-200 hover:text-white mr-4"
                >
                    Users
                </a>
                <a
                    href="/about"
                    className="block mt-4 lg:inline-block lg:mt-0 text-teal-200 hover:text-white mr-4"
                >
                    About
                </a> 
            </div>
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

### Assignment

**Get data from database and display it on the page**

**NOTE:** You need to be connected to Metropolia network or VPN to access the database.

1. Install [mysql2](https://www.npmjs.com/package/mysql2) and [dotenv](https://www.npmjs.com/package/dotenv) packages:
    ```bash
    npm install mysql2
    ```
2. Create `.env.local` file to the root of the project and add the following content:
    ```text
    DB_HOST=mysql.metropolia.fi
    DB_USER=your_username
    DB_PASS=your_password
    DB_NAME=your_username
    ```
3. Create `src/utils/db.ts` file and add the following content:
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
4. We are using the same database tables as earlier in this course. Download this [ZIP file]() and extract it. It
   contains a folder called `models` with a file
   called `media-model.ts`. Copy `models` folder to the `src` folder of the project.
5. Create new file `src/components/MediaList.tsx` and add the following content:
    ```tsx
    import { fetchAllMedia } from '@/models/media-model';
    
    const MediaList =  async () => {
      'use server';
      
      const mediaList = await fetchAllMedia();    
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
              <p>Date: {item.created_at.toLocaleDateString()}</p>
            </li>
           ))}
          </ul>
        </section>
      );
    };
    
    export default MediaList;
    ```
6. Modify `app/media/page.tsx` and add the following content:
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
7. Test the application in the browser.

---

## Next.js API Endpoints

API endpoints allow you to handle various types of HTTP requests, enabling data retrieval, modification, or any
server-side logic needed for your application.

### Creating API Endpoints in Next.js

- **`api` Directory:** To define API routes, you can create a special directory named `api` within your `app` folder.
- **File-Based Routing:** Each file you create a folder with `route.ts` (or js)  within the `api` directory
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

### Assignment

**Create a form and API endpoints to upload a media file**

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
          const formData = new FormData(e.currentTarget);
          const response = await fetchData('/api/media', {
            method: 'POST',
            body: formData,
          });
          console.log(response);
          router.refresh();
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
2. Modify `app/media/page.tsx` and add the following content:
    ```tsx
    import MediaList from '@/components/MediaList';
    import MediaForm from '@/components/MediaForm';
    
    const MediaPage = () => {
      return (
        <main>
          <h1 className="text-4xl font-bold">Media</h1>
          <MediaForm />
          <MediaList />
        </main>
      );
    };
    
    export default MediaPage;
    ```
3. Create new file `app/api/media/route.ts` and add the following content:
    ```ts
   import { addMedia } from '@/models/media-model';
   import { MediaItem, UploadResult } from '@/types/Types';
   import { fetchData } from '@/utils/functions';

   export async function POST(request: Request) {
   try {
     const formData = await request.formData();
     console.log(formData);

      const options = {
        method: 'POST',
        body: formData,
      };
   
      const result = await fetchData<UploadResult>(
        `${process.env.MEDIA_SERVER}/api/v1/upload`,
        options,
      );

      // check if the result is not valid
      if (!result.message || !result.data || !result.data.image) {
        return new Response('Invalid response from media server', {
          status: 500,
        });
      }
      // add data to database

      const { title, description, file } = Object.fromEntries(formData.entries());
      const { size, type } = file as File;
      const mediaItem: Omit<MediaItem, 'media_id' | 'created_at'> = {
        user_id: 4,
        filename: result.data.image,
        filesize: size,
        mediaType: type,
        title: title.toString(),
        description: description.toString(),
      };

      const addResult = await addMedia(mediaItem);
      console.log('addResult', addResult);
      result.message += ' ' + addResult.media_id;
      return new Response(JSON.stringify(result), {
        headers: { 'content-type': 'application/json' },
      });
     } catch (error) {
       console.error((error as Error).message);
       return new Response((error as Error).message, { status: 500 });
     }
   };
   ```
4. Test the application in the browser.

---

## What have we done so far?
1. Created a new Next.js project
2. Added routing to the project
3. Added a database connection to the project
4. Added data fetching to the project
5. Added API endpoints to the project
6. Added a form to the project
7. Used external API to upload a file to the server

## Things to think about
1. Why `MediaList` component is a server component and `MediaForm` component is a client component?
   - MediaList component is a server component because it is not interactive and can be cached and reused.
   - MediaForm component is a client component because forms are interactive and can be updated dynamically.
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

