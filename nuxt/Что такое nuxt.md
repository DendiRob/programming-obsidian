>[!info]
>Nuxt - это бесплатный open-source фреймворк с интуитивным и расширяемым способом создавать типо-безопасные, производительные и полноценные фуллстак веб-приложения и веб-сайты на  [[../vue/Vue|Vue]]

## [Automation and Conventions](https://nuxt.com/docs/getting-started/introduction#automation-and-conventions)

Nuxt использует соглашения и определенную структуру папок, чтобы избавиться от однообразных задач и дать возможность разработчику сфокусироваться на логике приложения. Также можно изменить поведение приложения с помощью конфигурационного файла.
-  **File-based routing:** роутинг основанный на структуре файлов, а именно папке __pages__. Это помогает легче организовать приложение и избежать ручной настройки путей
- **Code splitting:** Nuxt автоматически разбивает код на более маленькие части, чтобы уменьшить изначальное время загрузки приложения.
- **Server-side rendering out of the box:** Nuxt из-под коробки имеет возможности SSR, поэтому нет необходимости в настройке отдельного сервера
- **Auto-imports:** пишите Vue composables and components в соответствующих папках и используйте их без необходимости импорта, также есть оптимизатор js бандлов и tree-shaking.
- **Data-fetching utilities:** Nuxt provides composables to handle SSR-compatible data fetching as well as different strategies.
- **Zero-config TypeScript support:** пишите типо-безопасный код без необходимости изучения ТС с нашим авто сгенерированными типами и  `tsconfig.json`
- **Configured build tools:** we use [Vite](https://vitejs.dev/) by default to support hot module replacement (HMR) in development and bundling your code for production with best-practices baked-in.
## [Server-Side Rendering](https://nuxt.com/docs/getting-started/introduction#server-side-rendering)

Nuxt comes with built-in server-side rendering (SSR) capabilities by default, without having to configure a server yourself, which has many benefits for web applications:

- **Faster initial page load time:** Nuxt отправляет в браузер уже готовую HTML страницу, которая отображается мгновенно. Это улучшает пользовательский опыт, особенно, если у юзера слабый интернет сигнал или девай
- **Improved SEO:** поисковые движки лучше индексируют SSR pages, так как контент в них доступен сразу, в отличие от SPA, где сначала нужно выполнить JS код на стороне клиента.
- **Better performance on low-powered devices:** данная возможность уменьшает количество JS, необходимого для загрузки и выполнения клиентской стороны, это намного улучшает пользовательский опыт у юзер, которые обладают не самыми мощными девайсами, которым тяжело выполнять большое количество JS кода 
- **Better accessibility:** из-за того, что с сервера отправляется готовая html-страница, сайт становится более доступным для людей, которые пользуются скрин ридерами или другими вспомогательными устройствами.
- **Easier caching:** pages can be cached on the server-side, which can further improve performance by reducing the amount of time it takes to generate and send the content to the client.