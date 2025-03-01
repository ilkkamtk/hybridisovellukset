# Next.js - continued

---

## Optimizing

[Next.js Docs](https://nextjs.org/docs/app/building-your-application/optimizing)

---

### Assignment 1

Add images to `src/app/components/MediaList.tsx`.

1. Open `src/app/components/MediaList.tsx`. Add `import Image from 'next/image';` and then `<Image src={item.thumbnail} alt={item.title} width={320} height={200} />` to the `MediaList` component.
   - You'll get an error: `Error: Invalid src prop ... hostname "XXXX" is not configured under images in your next.config.js`. This is because Next.js requires you to configure the domains from which images are loaded.
2. Open `next.config.js` and add the following:

   ```javascript
   import type { NextConfig } from 'next';
   
   const nextConfig: NextConfig = {
     images: {
       remotePatterns: [
         {
           hostname: 'media2.edu.metropolia.fi',
         },
       ],
     },
   };
   ```

3. If you have images hosted on a different domain like localhost, Azure or Metropolia ecloud, you can add them to the `remotePatterns` array.

4. Create a page to display the full-size media when clicking on the thumbnail.

   - [Dynamic routes](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes)
   - Create a new file `src/app/single/[slug]/page.tsx`. Use the [example](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes#example) from the Next.js documentation to create a dynamic route for the media item. Use the `slug`to get the data of single media item from the database using `mediaModel`. Display the full-size media and the title. Use `Image` or `video` to display the media depending on the type. Also add a back button to return to the list.

5. Change all `<a>` tags to `<Link>` tags in your application. This will make the application faster and more responsive.

### Assignment 2

Add tags to the media items.

1. Modify the `MediaForm` to include a field for tags. You can use a simple text input for now. Later you can add a dropdown or autocomplete with predefined tags using e.g. `<datalist>`.

2. Create a new api route `src/app/api/tags/route.ts` to handle the tags. You can use the `src/app/api/media/route.ts` as a starting point. Instead of form data, use json data: `await reqest.json()`. Use `postTag()` function in `tagModel` to add the tag to the database. From the same function you can check what parmeters it needs and what it returns.

3. In `MediaForm` component, add a new function to handle the form submit. Use `fetchData()` to send the data to `api/tags`.

4. To add the tags to `mediaList` array in `mediaModel`, you need to use Promise.all to wait for all the tags to be fetched. You can use the `fetchTagsByMediaId()` function in `tagModel` to get the tags for each media item. Show the tags in the `MediaList` component. You can use a simple `<p>` for now.

### Building and deploying the application

1. Run `npm run build` to build the application. This will create a `.next` folder in the root of your project.

2. Run `npm run start` to start the application in production mode. Open your browser and go to `http://localhost:3000` to see the application running in production mode.

3. Deploy the application to Azure or Metropolia ecloud. For Azure you can use the same method as in the previous weeks. For Metroplia ecloud, the instructions are in Oma.

### Extra assignment, Infinite Scroll

Using this reapo as a guide, add infinite scroll to the `MediaList` component: [Infinite Scroll](https://github.com/adrianhajdin/anime_vault).

In our `mediaModel` you can add paramters to the `fetchAllMedia` function to limit the number of items returned and to skip the items already shown. Parameters are `page` and `limit`. First parameter is page and second is limit. For example, if you want to show 10 items per page, you call `fetchAllMedia(1, 10)` to get the first 10 items and `fetchAllMedia(2, 10)` to get the next 10 items.
