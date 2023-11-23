Несмотря на существование входных параметров и событий, иногда может потребоваться прямой доступ к дочернему компоненту в JavaScript. Для этого можно присвоить ID дочернему компоненту или HTML-элементу с помощью атрибута `ref`. Например:
```JS
<input ref="input" />
```

То есть,если нам  понадобиться изменить что-то в другом компоненте,например,обновить переменную или что-то другое,то мы можем использовать ref
# Разница в использовании ref
В Composition API нельзя свободно использовать ref, например, как в том же самом Option. Для этого в дочернем элементе следует указывать,какие именно данные могут быть изменены
## Option API
```JS
//Option API
<script> 
import ChildComponent from "@/components/ChildComponent.vue";  
export default {   
components: {     
	ChildComponent   
},   
methods: { 
	updateChildData() {       // Используем ref для доступа к дочернему компоненту и обновления данных в нем       
		this.$refs.childRef.updateData("Новые данные из родительского компонента");     
		}   
	} 
}; 
</script>
```

```JS
<!-- Дочерний компонент (ChildComponent.vue) --> >
<template>   
	<div>     
		<p>{{ childData }}</p>   
	</div> 
</template>  
<script> 
export default {   
	data() {     
		return {       
			childData: "Исходные данные"     
			};   
	},   
	methods: {     
		updateData(newData) {       // Обновляем данные в дочернем компоненте 
		      this.childData = newData;     
		      }   
		    } 
		}; 
		
</script>
```

## Composition API 

```JS
//Option API
<script setup> 
import ChildComponent from "@/components/ChildComponent.vue";
	function updateChildData() {       // Используем ref для доступа к дочернему компоненту и обновления данных в нем       
		this.$refs.childRef.updateData("Новые данные из родительского компонента");     
		}   
	}
}; 
</script>
```

```JS
<!-- Дочерний компонент (ChildComponent.vue) --> >
<template>   
	<div>     
		<p>{{ childData }}</p>   
	</div> 
</template>  
<script setup>
import childData  from 'vue';
const childData = ref('')
	defineExpose({childData}) //задаем методы и поля,которые можно изменить
</script>
```


#vue
