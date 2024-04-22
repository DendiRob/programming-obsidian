Pinia is a store library for Vue, it allows you to share a state across components/pages. If you are familiar with the Composition API, you might be thinking you can already share a global state with a simple `export const state = reactive({})`. This is true for single page applications but **exposes your application to [security vulnerabilities](https://vuejs.org/guide/scaling-up/ssr.html#cross-request-state-pollution)** if it is server side rendered. But even in small single page applications, you get a lot from using Pinia:

- Devtools support
    - A timeline to track actions, mutations
    - Stores appear in components where they are used
    - Time travel and easier debugging
- Hot module replacement
    - Modify your stores without reloading your page
    - Keep any existing state while developing
- Plugins: extend Pinia features with plugins
- Proper TypeScript support or **autocompletion** for JS users
- Server Side Rendering Support
[[../Vue]]
