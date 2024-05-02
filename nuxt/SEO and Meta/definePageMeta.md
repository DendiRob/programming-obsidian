
>[!info]
>`definePageMeta` is a compiler macro that you can use to set metadata for your **page** components located in the [`pages/`](https://nuxt.com/docs/guide/directory-structure/pages) directory (unless [set otherwise](https://nuxt.com/docs/api/nuxt-config#pages)). This way you can set custom metadata for each static or dynamic route of your Nuxt application.

>[!warning]
>Within your [`pages/` directory](https://nuxt.com/docs/guide/directory-structure/pages), you can use `definePageMeta` along with [`useHead`](https://nuxt.com/docs/api/composables/use-head) to set metadata based on the current route.
>For example, you can first set the current page title (this is extracted at build time via a macro, so it can't be set dynamically):

```JS
<script setup lang="ts">
definePageMeta({
  title: 'Some Page'
})
</script>
```

Полный список возможностей https://nuxt.com/docs/api/utils/define-page-meta
- Можно указывать layouts 