<template>
    <div>
      <h2>Daftar Tugas</h2>
      <div v-for="tugas in tugasList" :key="tugas.id">
        <to-do-item :todo="tugas" @toggle-status="toggleStatus" @hapus-item="hapusItem" @edit-item="editItem" />
      </div>
    </div>
  </template>
  
  <script>
  import ToDoItem from './ToDoItem.vue'; 
  
  export default {
    components: {
      ToDoItem
    },
    props: ['tugasList'],
    methods: {
      hapusItem(tugas) {
        this.$emit('hapus-item', tugas);
      },
      editItem(tugas) {
        this.$emit('edit-item', tugas);
      },
      toggleStatus(tugas) {
        this.$emit('toggle-status', tugas);
      }
    }
  }
  </script>
  