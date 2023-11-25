```JS
actions: {
    setActiveTab(id) {
      this.activeTab = id;
    }
},
```
Асинхронный action
```JS
actions: {
    async asyncFunc(id) {
      this.activeArr = await fetch('smth');
    }
},
```
[[pinia]]
