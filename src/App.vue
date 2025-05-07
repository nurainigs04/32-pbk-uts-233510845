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
const daftarKegiatan = ref([])
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
</style>
