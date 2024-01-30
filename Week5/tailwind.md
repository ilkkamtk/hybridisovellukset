# Tailwind CSS

## What is Tailwind CSS?

- Tailwind CSS is a utility-first CSS framework for building custom user interfaces.
    - Utility-firs means that you can use the classes to style your components. You don't need to write CSS yourself.
- It is not opinionated. Tailwind CSS doesn't tell you how your site should look like. You can use it to build your own
  design.
- However, it is not a UI kit. It doesn't provide you with ready-made components. You can use it to build your own
  components.
    - There are UI kits built with Tailwind CSS, for example [Tailwind UI](https://tailwindui.com/)
    - Some **not**-component libraries are [headlessui.dev](https://headlessui.dev/)
      and [shadcn/ui](https://ui.shadcn.com/). They can be used to build your own component library.
- [Core concepts](https://tailwindcss.com/docs/utility-first)

## Why Tailwind CSS?

- [This is why](https://www.youtube.com/watch?v=t-eR4hA7obg)

## Lab assignment 1

1. Continue last exercise. Create a new branch 'tailwind' with git.
2. [Install Tailwind CSS to your project.](https://tailwindcss.com/docs/guides/vite#react)
3. [Editor setup.](https://tailwindcss.com/docs/editor-setup)
    - Change `prettier.config.js` to:
    ```js
    export default {
       plugins: ["prettier-plugin-tailwindcss"],
    };
    ```
4. Rename `index.css` to `index_old.css`. Create a new blank `index.css` and add the following the beginning of the
   file:
    ```css
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
    ```
    - These are the [default styles](https://tailwindcss.com/docs/preflight) that Tailwind CSS provides.
5. Copy `:root` rule from `index_old.css` to `index.css` to get the basic styles back to the app.
6. Open `Layout.tsx` and add the same styles to the `Layout` component as you had before, but use Tailwind CSS classes
   instead of CSS.
    - Start with `ul` and `li` elements in `Nav` component. Use [Tailwind CSS docs](https://tailwindcss.com/docs) to
      find the right classes. The old styles are in `index_old.css` if you need to check them.
7. Do you really need to add the same styles to all `<li>` components? Isn't that repeating
   yourself? [Yes it is. And it is supposed to be like that.](https://tailwindcss.com/docs/reusing-styles#/dashboard)
   - You can however use pseudo classes like `*:` to [add styles to direct children](https://tailwindcss.com/docs/hover-focus-and-other-states#styling-direct-children) of E.g. `ul` element.
8. Go through `index_old.css` and make the app look like it did before (or better) with Tailwind CSS classes.
    - [Colors](https://tailwindcolor.com/)
    - [Default spacing](https://tailwindcss.com/docs/customizing-spacing#default-spacing-scale)
    - [Font size](https://tailwindcss.com/docs/font-size)

## Submit

1. Run `npm build` or `npm run build`
2. Move build folder to your public_html
3. Test your app: `http://users.metropolia.fi/~username/tailwind`
4. Modify README.md. Change the link in `Open [X](X) to view it in the browser.` to point to the above link.
5. git add, commit & push to remote repository
6. Submit the link to correct branch of your repository to Oma
