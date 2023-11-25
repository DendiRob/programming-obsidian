## Emitting and Listening to Events
https://ru.vuejs.org/v2/guide/components-custom-events.html
https://v3.ru.vuejs.org/ru/guide/component-custom-events.html

A component can emit custom events directly in template expressions (e.g. in a `v-on` handler) using the built-in `$emit` method:

```JS
<!-- MyComponent -->
<button @click="$emit('someEvent')">click me</button>
```

The parent can then listen to it using `v-on`:
```JS
<MyComponent @some-event="callback" />
```

The `.once` modifier is also supported on component event listeners:
```JS
<MyComponent @some-event.once="callback" />
```

Like components and props, event names provide an automatic case transformation. Notice we emitted a camelCase event, but can listen for it using a kebab-cased listener in the parent. As with [props casing](https://vuejs.org/guide/components/props#prop-name-casing), we recommend using kebab-cased event listeners in templates.

>[!info]
>Если нам нужно в Child компоненте использовать какое-то событие, которое будет изменять состояния или должно вызывать какую-то функцию в родителе, то мы можем использовать $emit, первый аргументов она принимает название события, а вторым данные, которые необходимо передать в родитель.
>А в родители мы навешиваем событие на компонент, а также создаем нужную функцию для его обработки, доки рекомендуют использовать кебаб синтаксис для назначения ивентов в родителе



