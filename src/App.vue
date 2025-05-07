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
    </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const kegiatanBaru = ref('')
const jamBaru = ref('')
const daftarKegiatan = ref([])

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
</script>


<style scoped>
</style>
