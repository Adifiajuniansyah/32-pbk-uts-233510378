<script setup>
import { ref } from 'vue'

// State daftar kegiatan (pakai objek agar bisa tandai selesai)
const activities = ref([
  { text: 'Belajar Vue.js', completed: false },
  { text: 'Mengerjakan tugas kuliah', completed: false },
  { text: 'Olahraga sore', completed: false },
  { text: 'Membaca buku', completed: false }
])

const newActivity = ref('')
const message = ref('')

// Fungsi untuk menambahkan kegiatan baru
const addActivity = () => {
  if (newActivity.value.trim() !== '') {
    activities.value.push({ text: newActivity.value, completed: false })
    message.value = `Kegiatan "${newActivity.value}" berhasil ditambahkan.`
    newActivity.value = ''

    setTimeout(() => {
      message.value = ''
    }, 3000)
  }
}

// Fungsi untuk menghapus kegiatan
const removeActivity = (index) => {
  const removed = activities.value.splice(index, 1)
  message.value = `Kegiatan "${removed[0].text}" telah dibatalkan.`

  setTimeout(() => {
    message.value = ''
  }, 3000)
}

// Fungsi untuk toggle selesai/tidak
const toggleCompleted = (index) => {
  activities.value[index].completed = !activities.value[index].completed
}
</script>

<template>
  <div class="container">
    <h1>Daftar Kegiatan</h1>

    <!-- Notifikasi -->
    <p v-if="message" class="message">{{ message }}</p>

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
      <li v-for="(activity, index) in activities" :key="index" class="activity-item">
        <span
          @click="toggleCompleted(index)"
          :class="{ completed: activity.completed }"
          class="activity-text"
        >
          {{ activity.text }}
        </span>
        <button class="delete-button" @click="removeActivity(index)">Hapus</button>
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

.message {
  background-color: #e0ffe0;
  border: 1px solid #00cc00;
  padding: 0.5em;
  margin-bottom: 1em;
  color: #006600;
  border-radius: 4px;
  text-align: center;
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

.activity-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 0.5em 0;
  list-style: disc;
}

.activity-text {
  cursor: pointer;
  transition: all 0.2s;
}

.activity-text.completed {
  text-decoration: line-through;
  color: #888;
}

.delete-button {
  background-color: #ffcccc;
  border: none;
  padding: 0.3em 0.6em;
  border-radius: 4px;
  color: #a00000;
  cursor: pointer;
  font-size: 0.9em;
}
</style>
