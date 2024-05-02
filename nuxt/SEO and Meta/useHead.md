>[!info]
>The [`useHead`](https://nuxt.com/docs/api/composables/use-head) composable function allows you to manage your head tags programmatically and reactively, powered by [Unhead](https://unhead.unjs.io/).

```JS
<script setup lang="ts">
useHead({
  title: 'My App',
  meta: [
    { name: 'description', content: 'My amazing site.' }
  ],
  bodyAttrs: {
    class: 'test'
  },
  script: [ { innerHTML: 'console.log(\'Hello world\')' } ]
})
</script>
```

![[../../Pasted image 20240502101513.png]]