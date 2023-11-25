## toRef()
Can be used to normalize values / refs / getters into refs (3.3+).
Can also be used to create a ref for a property on a source reactive object. The created ref is synced with its source property: mutating the source property will update the ref, and vice-versa.

Можно использоваться для создания [`ref`](https://vueframework.com/docs/v3/ru/ru/api/refs-api.html#ref) для свойства на исходном реактивном объекте. После этого ref-ссылку можно передавать, сохраняя реактивную связь с исходным свойством.

```JS
const state = reactive({
  foo: 1,
  bar: 2
})

const fooRef = toRef(state, 'foo')

fooRef.value++
console.log(state.foo) // 2

state.foo++
console.log(fooRef.value) // 3
```
`toRef` пригодится, когда требуется передать ref-ссылку из входного параметра в функцию композиции:

```JS
export default {
  setup(props) {
    useSomeFeature(toRef(props, 'foo'))
  }
}
```

`toRef` возвращает ref-ссылку пригодную для использования, даже если свойство в источнике в настоящее время не существует. Это особенно полезно при работе с необязательными входными параметрами, которые не будут подхвачены `toRefs` 

```JS
const cube = reactive({
	length:10,
	width;20
})
const {length, width} = cube.value // не будут реактивными, если мы захотим их изменить,то вью этого не заметит и реактивность не сработает
const length = toRef(cube, 'length')//А вот так всё будет работать при изменении значения


```

>[!info]
>Поможет после деструктуризации дать реактивность какому-то свойству объекта ,например,  пропсу!!!

## toRefs 
>[!info]
>Упрощает передачу реактивности свойствам объекта,
>то есть на не надо каждому свойству писать toRef, а достаточно объявить всему объекту(пример внизу)
>плюс дает возможность деструктуризации 



Преобразует реактивный объект в обычный объект, в котором каждое свойство будет `ref`, указывающей на соответствующее свойство исходного объекта.

```JS
const state = reactive({
  foo: 1,
  bar: 2
})

const stateAsRefs = toRefs(state)
/*
Тип stateAsRefs:

{
  foo: Ref<number>,
  bar: Ref<number>
}
*/

// Реактивная ref-ссылка и оригинальное свойство "связаны"
state.foo++
console.log(stateAsRefs.foo.value) // 2

stateAsRefs.foo.value++
console.log(state.foo) // 3
```

`toRefs` полезен при возвращении реактивного объекта из функции композиции, чтобы использующий компонент мог использовать деструктуризацию/оператор разложения к возвращаемому объекту без потери реактивности:

```JS
function useFeatureX() {
  const state = reactive({
    foo: 1,
    bar: 2
  })

  // логика, работающая с состоянием

  // при возвращении преобразуем к refs
  return toRefs(state)
}

export default {
  setup() {
    // теперь можно использовать деструктуризацию без потери реактивности
    const { foo, bar } = useFeatureX()

    return {
      foo,
      bar
    }
  }
}
```
`toRefs` будет генерировать ref-ссылки только для свойств, включённых в исходный объект. Для создания ref-ссылки для конкретного свойства следует использовать `toRef`

дело в том,что если есть объект реф,то его внутренние своиства не реактивны, toRefs помогает сделать их реактивными
## isRef()
Checks if a value is a ref object.
Данный метод проверяет, является ли значение реактивным
## unref()
Returns the inner value if the argument is a ref, otherwise return the argument itself. This is a sugar function for `val = isRef(val) ? val.value : val`.
Возвращает значение реф объекта ,если это аргумент не реф объект, то возвращает сам аргумент

```JS
let count = ref(1)
function double() {
count.value = unref(count) * 2 //будет умножаться, так как unraf превратила его в обычное значение и теперь коунт это не прокси объект
}
```

## shallowRef()
Создаёт ref-ссылку, которая отслеживает изменения своего `.value`, но не делает это значение реактивным.

```JS
const foo = shallowRef({})
// изменение значения ref-ссылки реактивно
foo.value = {}
// но значение не преобразуется в реактивное.
isReactive(foo.value) // false
```

## triggerRef
Выполняет любые эффекты, привязанные вручную к [`shallowRef`](https://vueframework.com/docs/v3/ru/ru/api/refs-api.html#shallowref).

```JS
const shallow = shallowRef({
  greet: 'Привет, мир'
})

// Выведет "Привет, мир" один раз при первом проходе
watchEffect(() => {
  console.log(shallow.value.greet)
})

// Это не вызовет эффект, потому что ref-ссылка неглубокая
shallow.value.greet = 'Привет, вселенная'

// Выведет "Привет, вселенная"
triggerRef(shallow)
```
[[Vue]]
