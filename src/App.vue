<template>
  <div>
    <h1>To Do List</h1>
    <input type="text" v-model="tugasBaru" @keyup.enter="tambahTugas" placeholder="Tambah tugas baru..." />
    <br>
    <div v-if="tugasBelumSelesai.length === 0">
      <p>Tidak ada tugas yang belum selesai.</p>
    </div>
    <div v-else>
      <h2>Daftar Tugas</h2>
      <br>
      <button @click="filterBelumSelesai">{{ tampilkanBelumSelesaiSaja ? 'Tampilkan Semua' : 'Tampilkan Belum Selesai Saja' }}</button>
      <br><br>
      <to-do-list :tugas-list="tugasBelumSelesai" @toggle-status="toggleStatus" @hapus-item="hapusTugas" @edit-item="editTugas" />
    </div>
  </div>
</template>

<script>
import ToDoList from './components/ToDoList.vue';

export default {
  data() {
    return {
      tugasBaru: '',
      daftarTugas: [],
      tampilkanBelumSelesaiSaja: false,
      editMode: false,
      editedTugas: null
    };
  },
  computed: {
    tugasBelumSelesai() {
      if (this.tampilkanBelumSelesaiSaja) {
        return this.daftarTugas.filter(tugas => !tugas.completed);
      }
      return this.daftarTugas;
    }
  },
  methods: {
    tambahTugas() {
      if (this.editMode) {
        this.editedTugas.text = this.tugasBaru;
        this.editMode = false;
        this.editedTugas = null;
      } else {
        if (this.tugasBaru.trim() !== '') {
          this.daftarTugas.push({ id: Date.now(), text: this.tugasBaru, completed: false });
          this.tugasBaru = '';
        }
      }
    },
    editTugas(tugas) {
      this.editMode = true;
      this.editedTugas = tugas;
      this.tugasBaru = tugas.text;
    },
    hapusTugas(tugas) {
      this.daftarTugas = this.daftarTugas.filter(item => item.id !== tugas.id);
    },
    filterBelumSelesai() {
      this.tampilkanBelumSelesaiSaja = !this.tampilkanBelumSelesaiSaja;
    },
    toggleStatus(tugas) {
      const index = this.daftarTugas.findIndex(item => item.id === tugas.id);
      if (index !== -1) {
        this.daftarTugas[index].completed = !this.daftarTugas[index].completed;
      }
    }
  },
  components: {
    ToDoList
  }
}
</script>

<style>
.completed {
  text-decoration: line-through;
}

body{
  background-color: grey;
}

/* Styling untuk judul */
h1 {
  font-size: 24px;
  color: #333;
  margin-bottom: 20px;
}

/* Styling untuk input tugas */
input[type="text"] {
  padding: 8px;
  margin-bottom: 10px;
  width: 300px;
}

/* Styling untuk tombol */
button {
  padding: 6px 12px;
  margin-left: 10px;
  cursor: pointer;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
}

button:hover {
  background-color: #45a049;
}

/* Styling untuk daftar tugas */
.task-item {
  margin-bottom: 10px;
}

.task-item label {
  cursor: pointer;
}

.completed {
  text-decoration: line-through;
}

/* Styling untuk pesan jika tidak ada tugas */
.no-tasks {
  color: #888;
}
</style>
