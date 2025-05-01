<script setup>
import { ref, computed, onMounted, watch } from 'vue'

// State utama
const activities = ref([])
const newActivity = ref('')
const message = ref('')
const showOnlyIncomplete = ref(false)

// Fungsi untuk memuat dari localStorage saat app dimuat
onMounted(() => {
  const saved = localStorage.getItem('activities')
  if (saved) {
    activities.value = JSON.parse(saved)
  } else {
    // Default data jika belum ada di localStorage
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

    <!-- Filter Kegiatan -->
    <div class="filter">
      <label>
        <input type="checkbox" v-model="showOnlyIncomplete" />
        Tampilkan hanya yang belum selesai
      </label>
    </div>

    <!-- Daftar Kegiatan -->
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
  margin-bottom: 1em;
}

input[type="text"] {
  flex: 1;
  padding: 0.5em;
  font-size: 1em;
}

button {
  padding: 0.5em 1em;
  font-size: 1em;
  cursor: pointer;
}

.filter {
  margin-bottom: 1.5em;
  text-align: center;
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
