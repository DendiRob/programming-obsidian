### Опция компонента setup 

Новая опция компонента `setup` выполняется **перед созданием компонента**, сразу после разрешения входных параметров `props`, и служит точкой старта для Composition API.

ВНИМАНИЕ!
Внутри `setup` не получится использовать `this`, потому что он не будет ссылаться на экземпляр компонента. Потому что вызов `setup` будет происходить до разрешения свойств `data`, `computed` или `methods`, а значит они будут недоступны.

Опция `setup` должна быть функцией, которая принимает аргументами `props` и `context` (о которых подробнее поговорим [дальше](https://v3.ru.vuejs.org/ru/guide/composition-api-setup.html#%D0%B0%D1%80%D0%B3%D1%83%D0%BC%D0%B5%D0%BD%D1%82%D1%8B)). Кроме того, всё что возвращается из функции `setup` будет доступно для остальных частей компонента (вычисляемых свойств, методов, хуков жизненного цикла и т.д.), а также в шаблоне компонента.

Добавим `setup` в компонент:

```JS
// src/components/UserRepositories.vue

export default {
  components: { RepositoriesFilters, RepositoriesSortBy, RepositoriesList },
  props: {
    user: {
      type: String,
      required: true
    }
  },
  setup(props) {
    console.log(props) // { user: '' }

    return {} // всё что возвращается, станет доступно в остальной части компонента
  }
  // ... остальная часть компонента
}
```

### Реактивные переменные с помощью `ref`

Во Vue 3 теперь можно сделать реактивную переменную где угодно с помощью новой функции `ref`, например так:

```JS
import { ref } from 'vue'

const counter = ref(0)
```

`ref` принимает аргумент и возвращает его обёрнутым в объект со свойством `value`, которое затем можно использовать для чтения или изменения значения реактивной переменной:

```
import { ref } from 'vue'

const counter = ref(0)

console.log(counter) // { value: 0 }
console.log(counter.value) // 0

counter.value++
console.log(counter.value) // 1
```

Оборачивание значения в объект может показаться лишним, но это нужно для одинакового поведения разных типов данных в JavaScript. Это связано с тем, что примитивные типы в JavaScript, такие как `Number` или `String`, передаются по значению, а не по ссылке:
Наличие объекта-обёртки вокруг любого значения позволяет безопасно использовать его в любой части приложения, не беспокоясь о потере реактивности где-то по пути. 

 В любом случае будет обновление, так как прокси обертка позволяет нам видеть изменения даже у объектов 
Примечание

Другими словами, `ref` создаёт **реактивную ссылку** к значению. Концепция работы со **ссылками** используется повсеместно в Composition API.

### Использование хуков жизненного цикла внутри `setup`

Для паритета возможностей между Composition API и Options API, также требовался способ использовать хуки жизненного цикла внутри `setup`. Это стало возможно благодаря новым функциям, экспортируемым Vue. Хуки жизненного цикла в Composition API именуются как и в Options API, но с префиксом `on`: т.е. `mounted` станет `onMounted`.

Эти функции принимают аргументом коллбэк, который выполнится при вызове компонентом хука жизненного цикла.

Добавим его в функцию `setup`:

```JS
// src/components/UserRepositories.vue; функция `setup`
import { fetchUserRepositories } from '@/api/repositories'
import { ref, onMounted } from 'vue'

// в компоненте
setup(props) {
  const repositories = ref([])
  const getUserRepositories = async () => {
    repositories.value = await fetchUserRepositories(props.user)
  }

  onMounted(getUserRepositories) // хук `mounted` вызовет `getUserRepositories`

  return {
    repositories,
    getUserRepositories
  }
}
```

### Автономные `computed`-свойства

Вне компонента Vue, как `ref` или `watch`, также можно создавать вычисляемые свойства с помощью функции `computed`, импортированной из Vue. Вернёмся к примеру со счётчиком:

```
import { ref, computed } from 'vue'

const counter = ref(0)
const twiceTheCounter = computed(() => counter.value * 2)

counter.value++
console.log(counter.value) // 1
console.log(twiceTheCounter.value) // 2
```

Функция `computed` вернёт **реактивную ссылку** _только для чтения_ с результатом коллбэка, который был передан первым аргументом в `computed`. Для доступа к **значению** такого вычисляемого свойства, нужно обращаться к свойству `.value`, как и в случае с `ref`.

[[Vue]]
