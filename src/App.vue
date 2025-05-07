<template>
  <div class="app">
      <h1>📝 Daftar Kegiatan Sehari-hari</h1>

      <form @submit.prevent="tambahKegiatan">
        <input v-model="kegiatanBaru" placeholder="Tambah kegiatan..." />
        <input v-model="jamBaru" type="time" />
        <button type="submit">➕ Tambah</button>
      </form>
    </div>
</template>

<script setup>
 
</script>


<style scoped>
</style>
