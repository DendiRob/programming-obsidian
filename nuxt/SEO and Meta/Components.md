Nuxt provides `<Title>`, `<Base>`, `<NoScript>`, `<Style>`, `<Meta>`, `<Link>`, `<Body>`, `<Html>` and `<Head>` components so that you can interact directly with your metadata within your component's template.

Because these component names match native HTML elements, they must be capitalized in the template.

`<Head>` and `<Body>` can accept nested meta tags (for aesthetic reasons) but this does not affect _where_ the nested meta tags are rendered in the final HTML.

```JS
<script setup lang="ts">
const title = ref('Hello World')
</script>

<template>
  <div>
    <Head>
      <Title>{{ title }}</Title>
      <Meta name="description" :content="title" />
      <Style type="text/css" children="body { background-color: green; }" ></Style>
    </Head>

    <h1>{{ title }}</h1>
  </div>
</template>

```