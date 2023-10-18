<script setup>
import { ref, computed } from 'vue'

// Variabel inisiasi

const nama = ref('');
const absensi = ref(0);
const tugas = ref(0);
const uts = ref(0);
const uas = ref(0);

// Variabel nilai

const nilaiAbsensi = computed(() => {
  return absensi.value / 16 * 100;
})
const nilaiTugas = computed(() => {
  return tugas.value;
})
const nilaiUts = computed(() => {
  return uts.value;
})
const nilaiUas = computed(() => {
  return uas.value;
})

// Perhitungan nilai akhir

const bobotAbsensi = 10;
const bobotTugas = 25;
const bobotUts = 30;
const bobotUas = 35;
const nilaiAkhir = computed(() => {
  const totalBobot = bobotAbsensi + bobotTugas + bobotUts + bobotUas;

  const nilaiAbsensiBobot = (nilaiAbsensi.value / 100) * bobotAbsensi;
  const nilaiTugasBobot = (nilaiTugas.value / 100) * bobotTugas;
  const nilaiUtsBobot = (nilaiUts.value / 100) * bobotUts;
  const nilaiUasBobot = (nilaiUas.value / 100) * bobotUas;

  return ((nilaiAbsensiBobot + nilaiTugasBobot + nilaiUtsBobot + nilaiUasBobot) / totalBobot) * 100;
})

const nilaiHuruf = computed(() => {
  return isInputValid.value ? (
    nilaiAkhir.value >= 85 && nilaiAkhir.value <= 100 ? 'A' :
    nilaiAkhir.value >= 80 && nilaiAkhir.value < 85 ? 'A-' :
    nilaiAkhir.value >= 75 && nilaiAkhir.value < 80 ? 'B+' :
    nilaiAkhir.value >= 70 && nilaiAkhir.value < 75 ? 'B' :
    nilaiAkhir.value >= 65 && nilaiAkhir.value < 70 ? 'B-' :
    nilaiAkhir.value >= 60 && nilaiAkhir.value < 65 ? 'C+' :
    nilaiAkhir.value >= 55 && nilaiAkhir.value < 60 ? 'C' :
    nilaiAkhir.value >= 50 && nilaiAkhir.value < 55 ? 'D' : 
    'F'
    ) : 'Input Invalid'
});

// Validasi input

const isInputValid = computed(() => {
  const isNamaValid = nama.value.trim() !== '';
  const isAbsensiValid = absensi.value >= 1 && absensi.value <= 16;
  const isTugasValid = tugas.value >= 1 && tugas.value <= 100;
  const isUtsValid = uts.value >= 1 && uts.value <= 100;
  const isUasValid = uas.value >= 1 && uas.value <= 100;

  return isNamaValid && isAbsensiValid && isTugasValid && isUtsValid && isUasValid;
});

</script>

<template>
  <div class="pt-12 font-sans">
    <div class="mx-auto max-w-md">
      <div class="overflow-hidden bg-white shadow-sm sm:rounded-lg">
        <div class="p-12 border-gray-200">
          <div class="mb-6">
            <h1 class="text-center font-extrabold text-2xl">Input Informasi</h1>
          </div>
          <div class="mb-6">
            <label for="nama" class="block text-sm font-bold mb-2">Nama:</label>
            <input v-model="nama" type="text" id="nama" name="nama" class="w-full px-3 py-2 border rounded-lg font-semibold focus:outline-none focus:border-blue-500" required>
            <p v-if="!isInputValid && (nama < 1)" class="text-red-600 mt-2">Mohon isi Nama</p>
          </div>
          <div class="mb-6">
            <label for="absensi" class="block text-sm font-bold mb-2">Total Absensi:</label>
            <input v-model="absensi" type="number" id="absensi" name="absensi" class="w-full px-3 py-2 border rounded-lg font-semibold focus:outline-none focus:border-blue-500" required>
            <p v-if="!isInputValid && (absensi < 1 || absensi > 16)" class="text-red-600 mt-2">Total Absensi berada dalam rentang 1 hingga 16.</p>
          </div>
          <div class="mb-6">
            <label for="tugas" class="block text-sm font-bold mb-2">Nilai Tugas:</label>
            <input v-model="tugas" type="number" id="tugas" name="tugas" class="w-full px-3 py-2 border rounded-lg font-semibold focus:outline-none focus:border-blue-500" required>
            <p v-if="!isInputValid && (tugas < 1 || tugas > 100)" class="text-red-600 mt-2">Nilai Tugas berada dalam rentang 1 hingga 100.</p>
          </div>
          <div class="mb-6">
            <label for="uts" class="block text-sm font-bold mb-2">Nilai UTS:</label>
            <input v-model="uts" type="number" id="uts" name="uts" class="w-full px-3 py-2 border rounded-lg font-semibold focus:outline-none focus:border-blue-500" required>
            <p v-if="!isInputValid && (uts < 1 || uts > 100)" class="text-red-600 mt-2">Nilai UTS berada dalam rentang 1 hingga 100.</p>
          </div>
          <div class="mb-6">
            <label for="uas" class="block text-sm font-bold mb-2">Nilai UAS:</label>
            <input v-model="uas" type="number" id="uas" name="uas" class="w-full px-3 py-2 border rounded-lg font-semibold focus:outline-none focus:border-blue-500" required>
            <p v-if="!isInputValid && (uas < 1 || uas > 100)" class="text-red-600 mt-2">Nilai UAS berada dalam rentang 1 hingga 100.</p>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="pb-12 mt-4 font-sans">
    <div class="mx-auto max-w-md">
      <div class="overflow-hidden bg-white shadow-sm sm:rounded-lg">
        <div class="p-12 border-gray-200">
          <div class="mb-6">
            <h1 class="text-center font-extrabold text-2xl">Hasil Perhitungan</h1>
          </div>
          <h1 class="text-center mb-8 text-xl font-extrabold">Nama: {{ nama }}</h1>
          <table>
            <thead>
              <tr class="font-bold">
                <th>
                  <h1>Mendapatkan Nilai Akhir</h1>
                </th>
                <th>
                  <h1>Mendapatkan Nilai Angka</h1>
                </th>
              </tr>
            </thead>
            <tbody>
              <tr class="text-center font-semibold">
                <td>
                  <h1>{{ nilaiAkhir.toFixed(2) }}</h1>
                </td>
                <td>
                  <h1>{{ nilaiHuruf }}</h1>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>
