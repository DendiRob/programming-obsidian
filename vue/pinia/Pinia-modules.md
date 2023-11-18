Модули в pinia позволяют нам пользоваться стейтами из разных модулей, причём импортировать мы можем так же стор
```JS
import { defineStore } from "pinia";
import { useMovieStore } from "./MovieStore";

export const useSearchStore = defineStore("searchStore", {
  state: () => ({
    loader: false,
    movies: [],
  }),
  actions: {
    addToUserMovies(object) {
      const movieStore = useMovieStore();//вызываем модуль стора
      movieStore.movies.push({ ...object, isWatched: false });//добавляем в его состояние новый фильм
      movieStore.activeTab = 1 //также меняем состояние
    },
  },
});
```
