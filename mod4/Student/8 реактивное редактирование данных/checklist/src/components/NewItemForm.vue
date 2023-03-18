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
