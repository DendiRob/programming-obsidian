Основное отличие между Composition API и Option API в том, что у первого более свободная структура написания кода, например, если во втором у тебя есть четкое разбиение на блоки типа methods, computed, wathch, data(), то в Composition API у тебя есть возможность свободно управлять и устанавливать переменные, методы, компьютеды в любом месте.

```JS
//Пример Composition API
<script setup>  
import { ref, computed, onMounted, watch } from ‘vue’;
 //reactive variable
const message = ref(‘Initial Message’);
//Method
const changeMessage = () => { 
  message.value = 'New Message!'; 
};
//computed
const capitalizedMessage = computed(() => { 
  return message.value.toUpperCase();
});
//watch
watch(message, (newValue, oldValue) => {
  console.log(`Message changed from "${oldValue}" to "${newValue}"`);
});
//mounted
onMounted(() => {
  console.log('Component mounted');
}
//props
const props = defineProps({
  title: String,
  likes: Number
})
//or
const props = defineProps(['foo'])
</script>
```

```JS
//Пример Option Api and setup
<script>
import smth from 'smth';
import { ref } from ‘vue’;
export default {
  data() {
    return {

    }
  },
  computed: {},
  methods: {},
  mounted(){},
  beforeUnmount(){},
  props: {
   title: { type: String, required: true },
  },
 setup(props) {
	 const anything = ref('sda')
    if (props.foo === isAbsent) {

    }
    return {
	    anything
    }
  }
}
</script>
```