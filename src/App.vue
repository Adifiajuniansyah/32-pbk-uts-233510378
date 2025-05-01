<script setup>
import { ref, computed, onMounted, watch } from 'vue'

// State utama
const activities = ref([])
const newActivity = ref('')
const message = ref('')
const showOnlyIncomplete = ref(false)

// Memuat data dari localStorage
onMounted(() => {
  const saved = localStorage.getItem('activities')
  if (saved) {
    activities.value = JSON.parse(saved)
  } else {
    activities.value = [
      { text: 'Belajar Vue.js', completed: false },
      { text: 'Mengerjakan tugas kuliah', completed: false },
      { text: 'Olahraga sore', completed: false },
      { text: 'Membaca buku', completed: false }
    ]
  }
})

// Simpan ke localStorage setiap kali `activities` berubah
watch(activities, (newVal) => {
  localStorage.setItem('activities', JSON.stringify(newVal))
}, { deep: true })

// Tambah kegiatan
const addActivity = () => {
  if (newActivity.value.trim() !== '') {
    activities.value.push({ text: newActivity.value, completed: false })
    message.value = `Kegiatan "${newActivity.value}" berhasil ditambahkan.`
    newActivity.value = ''

    setTimeout(() => { message.value = '' }, 3000)
  }
}

// Hapus kegiatan
const removeActivity = (index) => {
  const removed = filteredActivities.value[index]
  const originalIndex = activities.value.indexOf(removed)
  activities.value.splice(originalIndex, 1)
  message.value = `Kegiatan "${removed.text}" telah dibatalkan.`

  setTimeout(() => { message.value = '' }, 3000)
}

// Tandai selesai / belum selesai
const toggleCompleted = (index) => {
  const item = filteredActivities.value[index]
  const originalIndex = activities.value.indexOf(item)
  activities.value[originalIndex].completed = !activities.value[originalIndex].completed
}

// Filter kegiatan
const filteredActivities = computed(() => {
  return showOnlyIncomplete.value
    ? activities.value.filter(activity => !activity.completed)
    : activities.value
})
</script>

<template>
  <div class="container">
    <h1>Daftar Kegiatan</h1>

    <p v-if="message" class="message">{{ message }}</p>

    <div class="form">
      <input
        type="text"
        v-model="newActivity"
        placeholder="Masukkan kegiatan baru"
        @keyup.enter="addActivity"
      />
      <button @click="addActivity">Tambah</button>
    </div>

    <div class="filter">
      <label>
        <input type="checkbox" v-model="showOnlyIncomplete" />
        Tampilkan hanya yang belum selesai
      </label>
    </div>

    <ul>
      <li v-for="(activity, index) in filteredActivities" :key="index" class="activity-item">
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
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');

* {
  box-sizing: border-box;
}

body {
  font-family: 'Poppins', sans-serif;
  background-color: #f4f7fa;
}

.container {
  max-width: 600px;
  margin: 3em auto;
  padding: 2em;
  background: #ffffff;
  border-radius: 12px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  color: #333;
  margin-bottom: 1em;
}

.message {
  background-color: #d1fae5;
  border: 1px solid #10b981;
  padding: 0.75em;
  margin-bottom: 1em;
  color: #065f46;
  border-radius: 6px;
  text-align: center;
  font-weight: 500;
}

.form {
  display: flex;
  gap: 0.75em;
  margin-bottom: 1.5em;
}

input[type="text"] {
  flex: 1;
  padding: 0.75em;
  font-size: 1em;
  border: 1px solid #ccc;
  border-radius: 8px;
  outline: none;
  transition: 0.3s ease;
}

input[type="text"]:focus {
  border-color: #3b82f6;
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.2);
}

button {
  padding: 0.75em 1.2em;
  background-color: #3b82f6;
  color: white;
  border: none;
  border-radius: 8px;
  font-weight: 500;
  cursor: pointer;
  transition: 0.3s ease;
}

button:hover {
  background-color: #2563eb;
}

.filter {
  margin-bottom: 1.5em;
  text-align: center;
  color: #555;
  font-size: 0.95em;
}

ul {
  padding: 0;
  margin: 0;
  list-style: none;
}

.activity-item {
  background-color: #f9fafb;
  border: 1px solid #e5e7eb;
  border-radius: 8px;
  padding: 0.75em 1em;
  margin-bottom: 0.75em;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: background-color 0.3s;
}

.activity-item:hover {
  background-color: #f3f4f6;
}

.activity-text {
  cursor: pointer;
  font-size: 1em;
  transition: all 0.3s;
}

.activity-text.completed {
  text-decoration: line-through;
  color: #9ca3af;
}

.delete-button {
  background-color: #fecaca;
  color: #b91c1c;
  border: none;
  padding: 0.4em 0.8em;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.85em;
  font-weight: 500;
  transition: background-color 0.3s;
}

.delete-button:hover {
  background-color: #f87171;
  color: white;
}
</style>
