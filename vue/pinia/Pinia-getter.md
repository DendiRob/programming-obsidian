Используется как отображения свойств, а также для разных видов сортировок и прочего!!!!

Геттеры (getters) являются эквивалентом [вычисляемых свойств(computed)](https://vuejs.org/guide/essentials/computed.html) для состояния хранилища. Их можно определить с помощью свойства `getters` в `defineStore()`. Они принимают `state` как первый параметр при использовании стрелочных функций.

```JS
export const useCounterStore = defineStore('counter', { 
	state: () => ({ 
		count: 0,
	}), 
	getters: { 
		doubleCount: (state) => state.count * 2, }, 
	})
```
Использование обычных функции:
```JS
export const useMovieStore = defineStore("movieStore", {
  state: () => ({
    movies: [],
    activeTab: 1,
  }),
  getters: {
    watchedMovies() {
      return this.movies.filter((el) => el.isWatched);
    },
    totalCountMovies() {
      return this.movies.length;
    },
  },
});
```
В большинстве случаев геттеры полагаются только на состояние, однако, им может потребоваться использовать другие геттеры. Для этого мы можем получить доступ к _экземпляру всего хранилища_ через `this` при определении через обычную функцию, **но при этом необходимо определить тип возвращаемого значения (в TypeScript)**. Это связано с известным ограничением в TypeScript и **не затрагивает геттеры, определенные с помощью стрелочной функции, а также геттеры, не использующие `this`**:
```JS
export const useCounterStore = defineStore('counter', { 
state: () => ({
count: 0, 
}), 
getters: {  
	doubleCount(state) { 
		return state.count * 2 
	}, 
	doublePlusOne() {
		return this.doubleCount + 1 
		}, 
	}, 
})
```

Использование: Просто также вызываешь через стор