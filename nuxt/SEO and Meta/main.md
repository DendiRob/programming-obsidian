Одна из главных причин почему стоит использовать фреймворк nuxt - это его SEO оптимизация

Nuxt предоставляет нам несколько удобных способов задания SEO:

- nuxt.config.ts - это самый простой способ, который применяется ко всему приложению без возможности работы с реактивными данными
- [[definePageMeta]] - задает мета тэги(не может быть динамическим)
```JS
export default defineNuxtConfig({
  app: {
    head: {
      charset: 'utf-8',
      viewport: 'width=device-width, initial-scale=1',
    }
  }
})
```
- [[useHead]] - хук, который умеет определять seo для конкретной страницы, работать с реактивными переменными и вообще подходит для более серьезной работы с SEO
- [[useSeoMeta]] - The useSeoMeta composable lets you define your site's SEO meta tags as a flat object with full TypeScript support.
- [[Components]] - компонентный подход (Nuxt provides `<Title>`, `<Base>`, `<NoScript>`, `<Style>`, `<Meta>`, `<Link>`, `<Body>`, `<Html>` and `<Head>` components so that you can interact directly with your metadata within your component's template.)
> [!warning]
> В useHead, useSeoMeta, components можно использовать реактивные переменные, единственное, если мы пишем refVariable.value
> ```JS
> description: () => `description: ${title.value}`
> ```
> Иначе просто передаем ref переменную (пример снизу)

```JS
<script setup lang="ts">
const description = ref('My amazing site.')

useSeoMeta({
  description // так со всеми способами, где работает реактивность
})
</script>
```

### [External CSS](https://nuxt.com/docs/getting-started/seo-meta#external-css)
Также можно делать и в компонентах

```JS
<script setup lang="ts">
useHead({
  link: [
    {
      rel: 'preconnect',
      href: 'https://fonts.googleapis.com'
    },
    {
      rel: 'stylesheet',
      href: 'https://fonts.googleapis.com/css2?family=Roboto&display=swap',
      crossorigin: ''
    }
  ]
})
</script>

```