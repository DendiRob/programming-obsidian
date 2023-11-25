Returns the raw, original object of a Vue-created proxy.
Возвращает оригинальный объект от объекта прокси, используется , например, для мест, где нужно сравнить эти объекты
### Details

`toRaw()` can return the original object from proxies created by [`reactive()`](https://vuejs.org/api/reactivity-core#reactive), [`readonly()`](https://vuejs.org/api/reactivity-core#readonly), [`shallowReactive()`](https://vuejs.org/api/reactivity-advanced.html#shallowreactive) or [`shallowReadonly()`](https://vuejs.org/api/reactivity-advanced.html#shallowreadonly).

This is an escape hatch that can be used to temporarily read without incurring proxy access / tracking overhead or write without triggering changes. It is **not** recommended to hold a persistent reference to the original object. Use with caution.

## Example
```JS
const foo = {}
const reactiveFoo = reactive(foo)
console.log(toRaw(reactiveFoo) === foo)
```
[[Vue]]
