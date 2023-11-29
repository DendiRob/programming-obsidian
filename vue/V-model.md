V-model это синтаксический сахар, так называемая надстройка, которая позволяет код
Можем проследить её функционал на примере двух сторонней связки во Vue 2 и Vue3
## Vue 2
https://ru.vuejs.org/v2/guide/components-custom-events.html
```JS
//input component
<script>
	name: 'InputChild',
	props: {
		 value: String
	 }

</script> 
<template> 
	  <input type="text" value="value" @input="$emit('input', $event.target.value)"/>
		//Мы принимаем value из пропсов,оно автоматом подставляется, а чтобы изменять в родителе переменную, то мы используем событие инпут, на каждый вызов которого, происходит эмит события у парента, то есть парент подразумевает событие инпут сверху. 
</template> 
<style scoped> 
	button { 
		font-weight: bold; 
	} 
</style>
```

```JS
//parent
<script>
import InputChild from './InputChild.vue'
	components: {
		InputChild
	}
	data(){
		return {
			reactiveValue: ''
		}
	}

</script> 
<template> 
		<InputChild v-model="reactiveValue" />
		//Как я уже сказал сверху, v-model это просто синтаксический сахар, он заменяет следующее
	<InputChild :value="reactiveValue" @input="reactiveValue = $event.target.value"/>
</template> 
<style scoped> 
	button { 
		font-weight: bold; 
	} 
</style>
```


## Vue 3
https://v3.ru.vuejs.org/ru/guide/component-custom-events.html#стиль-именования-событии
```JS
//input component
<script setup>
defineProps(['modelValue'])
defineEmits(['update:modelValue'])

</script> 
<template> 
	 <input @input="$emit('update:modelValue', $event.target.value)" :value="modelValue" placeholder="Search" type="text" class="search__input">
		//здесь мы уже принимаем modelValue и эмитируем другое событие 
</template> 
<style scoped> 
	button { 
		font-weight: bold; 
	} 
</style>
```

```JS
//parrent
<script setup>
import InputChild from './InputChild.vue'
const reactiveValue = ref('')

</script> 
<template> 
		<InputChild v-model="reactiveValue" />
		//Как я уже сказал сверху, v-model это просто синтаксический сахар, он заменяет следующее
	<InputChild :modelValue="reactiveValue" @update:modelValue="reactiveValue = $event.target.value"/>
</template> 
<style scoped> 
	button { 
		font-weight: bold; 
	} 
</style>
```

так же если мы хотим костюмной назвать модель, то можем использовать такой синтаксис
```JS
<InputChild v-model:customValue="reactiveValue" />
// соответсвенно мы меняем и событие
@update:customValue

```

