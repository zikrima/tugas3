<template>
    <div>
      <input type="checkbox" v-model="todo.completed" @change="toggleStatus" />
      <span :style="{ 'text-decoration': todo.completed ? 'line-through' : 'none' }">{{ todo.text }}</span>
      <button @click="editTugas">Edit</button>
      <button @click="hapusTugas">Hapus</button>
    </div>
  </template>
  
  <script>
  export default {
    props: ['todo'],
    methods: {
      toggleStatus() {
        this.$emit('toggle-status', this.todo);
      },
      hapusTugas() {
        this.$emit('hapus-item', this.todo);
      },
      editTugas() {
        this.$emit('edit-item', this.todo);
      }
    }
  }
  </script>
  
  <style scoped>
  span {
    cursor: pointer;
  }
  </style>
  