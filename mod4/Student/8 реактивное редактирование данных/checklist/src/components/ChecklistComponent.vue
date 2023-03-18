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