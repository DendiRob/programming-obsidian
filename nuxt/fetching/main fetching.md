>[!info]
>Nuxt comes with two composables and a built-in library to perform data-fetching in browser or server environments: `useFetch`, [`useAsyncData`](https://nuxt.com/docs/api/composables/use-async-data) and `$fetch`.

In a nutshell:
- [[useFetch]] is the most straightforward way to handle data fetching in a component setup function.
- [[$fetch]] is great to make network requests based on user interaction.
- [[useAsyncData]], combined with `$fetch`, offers more fine-grained control.

Both `useFetch` and `useAsyncData` share a common set of options and patterns that we will detail in the last sections.