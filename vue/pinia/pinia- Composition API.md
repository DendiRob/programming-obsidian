Сравнение синтаксисов одного и того же стора
```JS
//OptionAPI
import { defineStore } from "pinia";
import { useMovieStore } from "./MovieStore";

export const useSearchStore = defineStore("searchStore", {
  state: () => ({
    loader: false,
    movies: [],
  }),
  actions: {
    async getMovies(search) {
      this.loader = true;
      const res = await fetch(`${url}${search}`);
      const data = await res.json();
      this.movies = data.results;
      this.loader = false;
    },
    addToUserMovies(object) {
      const movieStore = useMovieStore();
      movieStore.movies.push({ ...object, isWatched: false });
      movieStore.activeTab = 1;
    },
  },
});
```

```JS
//CompositionAPI

import { defineStore } from "pinia";
import { useMovieStore } from "./MovieStore";
import { ref } from "vue";

export const useSearchStore = defineStore("searchStore", () => {
  const loader = ref(false);
  const movies = ref([]);

  const getMovies = async (search) => {
    loader.value = true;
    const res = await fetch(`${url}${search}`);
    const data = await res.json();
    movies.value = data.results;
    loader.value = false;
  };

  const addToUserMovies = (object) => {
    const movieStore = useMovieStore();
    movieStore.movies.push({ ...object, isWatched: false });
    movieStore.activeTab = 1;
  };

  return {
    loader,
    movies,
    getMovies,
    addToUserMovies,
  };
});
```