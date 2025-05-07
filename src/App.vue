<template>
  <div class="app">
      <h1>📝 Daftar Kegiatan Sehari-hari</h1>

      <form @submit.prevent="tambahKegiatan">
        <input v-model="kegiatanBaru" placeholder="Tambah kegiatan..." />
        <input v-model="jamBaru" type="time" />
        <button type="submit">➕ Tambah</button>
      </form>

      <div class="filter-container">
      <button 
        @click="filterStatus = 'semua'" 
        :class="{ active: filterStatus === 'semua' }"
        class="filter-btn"
      >
        Semua
      </button>
      <button 
        @click="filterStatus = 'aktif'" 
        :class="{ active: filterStatus === 'aktif' }"
        class="filter-btn"
      >
        Belum Selesai
      </button>
      <button 
        @click="filterStatus = 'selesai'" 
        :class="{ active: filterStatus === 'selesai' }"
        class="filter-btn"
      >
        Selesai
      </button>
    </div>

    <div v-if="filteredKegiatan.length === 0" class="empty-state">
      Tidak ada kegiatan {{ filterStatusText }}
    </div>

    <ul v-else>
      <li
        v-for="(item, index) in filteredKegiatan"
        :key="index"
        class="list-item"
      >
        <label>
          <input type="checkbox" v-model="item.selesai" />
          <span :class="{ selesai: item.selesai }">
            {{ item.nama }} - {{ item.jam }}
          </span>
        </label>
        <button @click="hapusKegiatan(daftarKegiatan.indexOf(item))" class="hapus-btn">🗑️</button>
      </li>
    </ul>

    <div class="status-bar">
      <span>Total: {{ daftarKegiatan.length }} kegiatan</span>
      <span>Selesai: {{ kegiatanSelesai.length }}</span>
      <span>Belum Selesai: {{ kegiatanAktif.length }}</span>
    </div>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const kegiatanBaru = ref('')
const jamBaru = ref('')
const daftarKegiatan = ref([
  {
    nama: "Belajar Vue.js",
    jam: "10:00",
    selesai: true
  },
  {
    nama: "Mengerjakan proyek",
    jam: "14:30",
    selesai: false
  }
])
const filterStatus = ref('semua')

const kegiatanSelesai = computed(() => {
  return daftarKegiatan.value.filter(item => item.selesai)
})

const kegiatanAktif = computed(() => {
  return daftarKegiatan.value.filter(item => !item.selesai)
})

const filteredKegiatan = computed(() => {
  if (filterStatus.value === 'aktif') {
    return kegiatanAktif.value
  } else if (filterStatus.value === 'selesai') {
    return kegiatanSelesai.value
  } else {
    return daftarKegiatan.value
  }
})

const filterStatusText = computed(() => {
  if (filterStatus.value === 'aktif') {
    return 'yang belum selesai'
  } else if (filterStatus.value === 'selesai') {
    return 'yang sudah selesai'
  } else {
    return ''
  }
})

function tambahKegiatan() {
  if (kegiatanBaru.value.trim() === '' || jamBaru.value.trim() === '') return

  daftarKegiatan.value.push({
    nama: kegiatanBaru.value,
    jam: jamBaru.value,
    selesai: false
  })

  kegiatanBaru.value = ''
  jamBaru.value = ''
}

function hapusKegiatan(index) {
  daftarKegiatan.value.splice(index, 1)
}
</script>


<style scoped>
.app {
  max-width: 500px;
  margin: 2rem auto;
  padding: 1rem 1.5rem;
  background: linear-gradient(135deg, #f0f4ff, #d0e4ff);
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

h1 {
  text-align: center;
  color: #333;
  margin-bottom: 1rem;
}

form {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

input[type='text'],
input[type='time'] {
  padding: 0.5rem;
  flex: 1;
  border: 1px solid #ccc;
  border-radius: 6px;
  outline: none;
  transition: border-color 0.3s;
}

input[type='text']:focus,
input[type='time']:focus {
  border-color: #0077ff;
}

button {
  padding: 0.5rem 1rem;
  background-color: #0077ff;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.1s;
}

button:hover {
  background-color: #005fcc;
}

button:active {
  transform: scale(0.95);
}

.filter-container {
  display: flex;
  gap: 0.5rem;
  margin: 1rem 0;
  justify-content: center;
}

.filter-btn {
  background-color: #e0e0e0;
  color: #333;
  font-size: 0.9rem;
  padding: 0.3rem 0.75rem;
}

.filter-btn:hover {
  background-color: #d0d0d0;
}

.filter-btn.active {
  background-color: #0077ff;
  color: white;
}

ul {
  list-style: none;
  padding: 0;
}

.list-item {
  background: white;
  padding: 0.75rem;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 2px 5px rgba(0,0,0,0.05);
  margin-bottom: 0.5rem;
}

.list-item label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  flex: 1;
  min-width: 0;
}

.list-item span {
  font-size: 1rem;
  color: #333; 
  word-break: break-word;
  flex: 1;
}

.selesai {
  text-decoration: line-through;
  color: gray !important; 
}

.hapus-btn {
  background: transparent;
  color: #ff4d4d;
  padding: 0.25rem;
  border: none;
  cursor: pointer;
  font-size: 1.2rem;
  transition: transform 0.2s ease;
}

.hapus-btn:hover {
  transform: scale(1.2);
}

.status-bar {
  display: flex;
  justify-content: space-between;
  margin-top: 1rem;
  padding-top: 0.75rem;
  border-top: 1px solid #ccc;
  font-size: 0.85rem;
  color: #555;
}

.empty-state {
  text-align: center;
  padding: 1.5rem;
  color: #777;
  font-style: italic;
  background: rgba(255, 255, 255, 0.7);
  border-radius: 8px;
  margin: 1rem 0;
}
</style>
