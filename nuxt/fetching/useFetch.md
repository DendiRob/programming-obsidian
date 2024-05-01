>[!info]
>This composable provides a convenient wrapper around [`useAsyncData`](https://nuxt.com/docs/api/composables/use-async-data) and [`$fetch`](https://nuxt.com/docs/api/utils/dollarfetch). It automatically generates a key based on URL and fetch options, provides type hints for request url based on server routes, and infers API response type.

## [ Usage](https://nuxt.com/docs/api/composables/use-fetch#usage)

```JS
<script setup lang="ts">
const { data, pending, error, refresh } = await useFetch('/api/modules', {
  pick: ['title']
})
</script>
```

>[!info]
>`data`, `pending`, `status` and `error` are Vue refs and they should be accessed with `.value` when used within the `<script setup>`, while `refresh`/`execute` is a plain function for refetching data.

Using the `query` option, you can add search parameters to your query. This option is extended from [unjs/ofetch](https://github.com/unjs/ofetch) and is using [unjs/ufo](https://github.com/unjs/ufo) to create the URL. Objects are automatically stringified.

```JS
const param1 = ref('value1')
const { data, pending, error, refresh } = await useFetch('/api/modules', {
  query: { param1, param2: 'value2' }
})
```

_The above example results in `https://api.nuxt.com/modules?param1=value1&param2=value2`._

You can also use [interceptors](https://github.com/unjs/ofetch#%EF%B8%8F-interceptors):

```JS
const { data, pending, error, refresh } = await useFetch('/api/auth/login', {
  onRequest({ request, options }) {
    // Set the request headers
    options.headers = options.headers || {}
    options.headers.authorization = '...'
  },
  onRequestError({ request, options, error }) {
    // Handle the request errors
  },
  onResponse({ request, response, options }) {
    // Process the response data
    localStorage.setItem('token', response._data.token)
  },
  onResponseError({ request, response, options }) {
    // Handle the response errors
  }
})
```

## Итог
Грубо useFetch это надстройка (синтаксический сахар) над [[useAsyncData]], это синтаксический саха позволяет нам отслеживать реактивные параметры без всяких вотчеров и тд.

>[!example]
> ```JS
> const { data } = await useFetch('url') // тоже, что и const {data} = useAsyncData('url', async () => await $fetch('url'))

## Другие опции и параметры 
![[../../imgs/Pasted image 20240501172259.png]]
![[../../imgs/Pasted image 20240501172318.png]]