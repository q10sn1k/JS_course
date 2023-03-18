# Домашнее задание:

### Задача 1

Необходимо разработать компонент "Чеклист" на Vue.js. Компонент должен представлять собой список задач, каждая из которых имеет возможность отмечаться как выполненная и удаляться. Также должна быть возможность добавления новых задач.

#### Решение


src/components/ChecklistComponent.vue

```vue
<template>
  <div>
    <h1>{{ title }}</h1>
    <ul>
      <checklist-item
        v-for="(item, index) in items"
        :key="index"
        :item="item"
        @toggle="toggleItem(index)"
        @delete="deleteItem(index)"
      ></checklist-item>
    </ul>
    <new-item-form @add="addItem"></new-item-form>
  </div>
</template>

<script>
import ChecklistItem from './ChecklistItem.vue'
import NewItemForm from './NewItemForm.vue'

export default {
  name: 'ChecklistComponent',
  components: {
    ChecklistItem,
    NewItemForm
  },
  data() {
    return {
      title: 'My Checklist',
      items: [
        { text: 'Task 1', done: false },
        { text: 'Task 2', done: false },
        { text: 'Task 3', done: false },
      ]
    }
  },
  methods: {
    toggleItem(index) {
      const item = this.items[index]
      if (item && item.done !== undefined) {
        item.done = !item.done
      } else {
        this.items[index].done = false
      }
    },
    deleteItem(index) {
      this.items.splice(index, 1)
    },
    addItem(text) {
      this.items.push({ text, done: false })
    }
  }
}
</script>

```

src/components/ChecklistItem.vue
```vue
<template>
  <li :class="{ 'done': localItem.done }">
    <input type="checkbox" v-model="localItem.done" @change="toggle" />
    <span>{{ localItem.text }}</span>
    <button @click="deleteItem">Delete</button>
  </li>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      localItem: Object.assign({}, this.item)
    }
  },
  methods: {
    toggle() {
      this.$emit('toggle')
    },
    deleteItem() {
      this.$emit('delete')
    }
  },
  watch: {
    item(newValue) {
      this.localItem = Object.assign({}, newValue)
    }
  }
}
</script>

```

src/components/NewItemForm.vue

```vue
<template>
  <form @submit.prevent="addItem">
    <input type="text" v-model="text" placeholder="Add a new item..." />
    <button type="submit">Add</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      text: ''
    }
  },
  methods: {
    addItem() {
      if (this.text.trim()) {
        this.$emit('add', this.text.trim())
        this.text = ''
      }
    }
  }
}
</script>

```

src/App.vue

```vue
<template>
  <div id="app">
    <checklist></checklist>
  </div>
</template>

<script>
import Checklist from './components/ChecklistComponent.vue'

export default {
  components: {
    Checklist
  }
}
</script>

```

src/main.js
```js
import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')

```

public/index.html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Vue Checklist</title>
</head>
<body>
<div id="app"></div>
</body>
</html>

```