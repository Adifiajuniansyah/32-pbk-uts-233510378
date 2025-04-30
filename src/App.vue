<script setup>
import { ref } from 'vue'

// State daftar kegiatan
const activities = ref([
  'Belajar Vue.js',
  'Mengerjakan tugas kuliah',
  'Olahraga sore',
  'Membaca buku'
])

// State untuk input kegiatan baru
const newActivity = ref('')

// Fungsi untuk menambahkan kegiatan baru
const addActivity = () => {
  if (newActivity.value.trim() !== '') {
    activities.value.push(newActivity.value)
    newActivity.value = ''
  }
}
</script>

<template>
  <div class="container">
    <h1>Daftar Kegiatan</h1>

    <!-- Form Tambah Kegiatan -->
    <div class="form">
      <input
        type="text"
        v-model="newActivity"
        placeholder="Masukkan kegiatan baru"
        @keyup.enter="addActivity"
      />
      <button @click="addActivity">Tambah</button>
    </div>

    <!-- Daftar Kegiatan -->
    <ul>
      <li v-for="(activity, index) in activities" :key="index">
        {{ activity }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
.container {
  max-width: 600px;
  margin: auto;
  padding: 2em;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  margin-bottom: 1em;
}

.form {
  display: flex;
  gap: 0.5em;
  margin-bottom: 1.5em;
}

input {
  flex: 1;
  padding: 0.5em;
  font-size: 1em;
}

button {
  padding: 0.5em 1em;
  font-size: 1em;
  cursor: pointer;
}

ul {
  padding-left: 1.2em;
}

li {
  margin: 0.5em 0;
  list-style: disc;
}
</style>
