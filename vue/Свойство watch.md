Данное свойство необходимо для работы с переменными, например, производить логику если отслеживаемая переменная получила нужное значение
```JS
//optionAPI
<script>
  export default {
    data() {
      return {
        notes: 0
      }
    },
    methods: {
      addNote() {
        this.notes += 1;
      }
    },
    watch: {
      notes(oldValue,newValue) { //называем также как и своиство за которым следим
        if(this.notes === 5){
          this.notes = 0
        }
      }
    }
  }
</script>
```

```JS
<script setup> 
import { ref, watch } from 'vue'; 
	const count = ref(0); 
	watch(count, (newCount, oldCount) => { 
	console.log(`Count changed from ${oldCount} to ${newCount}`); 
	}); 
	const increment = () => { 
		count.value++; 
	};
};
</script>
```

[[pinia/pinia]] 
[[Vue]]
