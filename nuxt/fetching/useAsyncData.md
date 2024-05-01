>[!info]
>__useAsyncData__ - утилита, которая помогает нам работать с асинхронными данными, первый аргумент это - key (a unique key to ensure that data fetching can be properly de-duplicated across requests. If you do not provide a key, then a key that is unique to the file name and line number of the instance of `useAsyncData` will be generated for you.)
>второй это коллбэк, который уже может делать разные запросы и логику

```JS
<script setup lang="ts">
const page = ref(1)
const { data: posts } = await useAsyncData(
  'posts',
  () => $fetch('https://fakeApi.com/posts', { // здесь может быть несколько запросов и тд
    params: {
      page: page.value
    }
  }), {
    watch: [page] // приходится явно указывать за какой реактивной переменной следить
  }
)
</script>
```

## Другие опции и параметры 

![[../../imgs/Pasted image 20240501172145.png]]