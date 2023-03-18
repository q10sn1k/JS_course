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
