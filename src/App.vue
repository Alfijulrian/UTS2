<template>
  <div class="container">
    <h1>🩺 Data Pasien Harian - Bidan R. Eriani</h1>

    <div class="input-group">
      <input v-model="namaPasien" placeholder="Nama Pasien" />
      <input v-model="penyakitPasien" placeholder="Penyakit" />
      <button @click="tambahPasien">Tambah</button>
    </div>

    <div class="filter">
      <label>
        <input type="checkbox" v-model="tampilkanBelumDitangani" />
        Tampilkan hanya pasien yang belum ditangani
      </label>
    </div>

    <ul class="patient-list">
      <li v-for="(pasien, index) in pasienDitampilkan" :key="index">
        <input type="checkbox" v-model="pasien.ditangani" />
        <span :class="{ done: pasien.ditangani }">
          {{ pasien.nama }} - {{ pasien.penyakit }}
        </span>
        <button @click="hapusPasien(index)">Hapus</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const namaPasien = ref('')
const penyakitPasien = ref('')
const pasienList = ref([])
const tampilkanBelumDitangani = ref(false)

const tambahPasien = () => {
  if (namaPasien.value.trim() && penyakitPasien.value.trim()) {
    pasienList.value.push({
      nama: namaPasien.value.trim(),
      penyakit: penyakitPasien.value.trim(),
      ditangani: false
    })
    namaPasien.value = ''
    penyakitPasien.value = ''
  }
}

const hapusPasien = (index) => {
  pasienList.value.splice(index, 1)
}

const pasienDitampilkan = computed(() => {
  return tampilkanBelumDitangani.value
    ? pasienList.value.filter(p => !p.ditangani)
    : pasienList.value
})
</script>

<style scoped>
.container {
  max-width: 650px;
  margin: auto;
  padding: 2rem;
  background: #f4fffb;
  border-radius: 16px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.06);
  font-family: 'Segoe UI', sans-serif;
  color: #2d3e50;
}

h1 {
  text-align: center;
  color: #208f84;
  margin-bottom: 1.5rem;
  font-weight: bold;
}

.input-group {
  display: flex;
  gap: 0.5rem;
  flex-wrap: wrap;
  margin-bottom: 1rem;
}

input[type="text"] {
  flex: 1;
  padding: 0.6rem;
  border-radius: 8px;
  border: 1px solid #c0e5e0;
  background: #ffffff;
}

button {
  padding: 0.6rem 1.2rem;
  background: #20b486;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.3s ease;
}
button:hover {
  background: #179b73;
}

.patient-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.patient-list li {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.7rem 0;
  border-bottom: 1px solid #e0f0ec;
}

input[type="checkbox"] {
  transform: scale(1.2);
}

.done {
  text-decoration: line-through;
  color: #8d8d8d;
}

.filter {
  text-align: center;
  margin-bottom: 1rem;
  color: #208f84;
}

@media (max-width: 480px) {
  .input-group {
    flex-direction: column;
  }
}
</style>
