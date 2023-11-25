Запуск store :
Создаем отдельный стор, который будет использоваться как хук в компонентах, использующих этот стор!
```JS
import { defineStore } from "pinia";

export const useMovieStore = defineStore("movieStore", {
  state: () => ({
    movies: [],
    activeTab: 1,
  }),
});
```

Подключение глобального стора к Vue:
```JS
import { createPinia } from "pinia";
import { createApp } from "vue";

import App from "./App.vue";

import "./assets/main.css";

createApp(App).use(createPinia()).mount("#app");

```
Использование Pinia в компонентах 
```JS
<script setup>
import Movie from "./components/Movie.vue";
import { useMovieStore } from "./stores/MovieStore";

const movieStore = useMovieStore(); //теперь можно обращаться как к объекту стор, причем и к гетера и ко всему
</script>
```
[[pinia]]
