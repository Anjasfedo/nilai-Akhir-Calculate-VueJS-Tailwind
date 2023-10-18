<script setup>
import { ref, computed } from 'vue'

const nama = ref('');
const absensi = ref(0);
const tugas = ref(0);
const uts = ref(0);
const uas = ref(0);

const isInputValid = computed(() => {
  const isNamaValid = nama.value.trim() !== '';
  const isAbsensiValid = absensi.value >= 1 && absensi.value <= 16;
  const isTugasValid = tugas.value >= 1 && tugas.value <= 100;
  const isUtsValid = uts.value >= 1 && uts.value <= 100;
  const isUasValid = uas.value >= 1 && uas.value <= 100;

  return isNamaValid && isAbsensiValid && isTugasValid && isUtsValid && isUasValid;
});

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
  return isInputValid.value ? 
    (nilaiAkhir.value >= 85 && nilaiAkhir.value <= 100 ? 'A' :
    nilaiAkhir.value >= 80 && nilaiAkhir.value <= 84 ? 'A-' :
    nilaiAkhir.value >= 75 && nilaiAkhir.value <= 79 ? 'B+' :
    nilaiAkhir.value >= 70 && nilaiAkhir.value <= 74 ? 'B' :
    nilaiAkhir.value >= 65 && nilaiAkhir.value <= 69 ? 'B-' :
    nilaiAkhir.value >= 60 && nilaiAkhir.value <= 64 ? 'C+' :
    nilaiAkhir.value >= 55 && nilaiAkhir.value <= 59 ? 'C' :
    nilaiAkhir.value >= 50 && nilaiAkhir.value <= 54 ? 'D' : 'F') : 'Input Invalid'
});
</script>

<template>
  <div class="pt-12">
    <div class="mx-auto max-w-2xl">
      <div class="overflow-hidden bg-white shadow-sm sm:rounded-lg">
        <div class="p-12 bg-white border-b border-gray-200">
          <div class="mb-4">
            <label for="nama" class="block text-gray-600 text-sm font-semibold mb-2">Nama:</label>
            <input v-model="nama" type="text" id="nama" name="nama" class="w-full px-3 py-2 border rounded-lg text-gray-700 focus:outline-none focus:border-blue-500" required>
            <p v-if="!isInputValid && (nama < 1)" class="text-red-500">Nama Harus Diisi</p>
          </div>
          <div class="mb-4">
            <label for="absensi" class="block text-gray-600 text-sm font-semibold mb-2">Absensi:</label>
            <input v-model="absensi" type="number" id="absensi" name="absensi" class="w-full px-3 py-2 border rounded-lg text-gray-700 focus:outline-none focus:border-blue-500" required>
            <p v-if="!isInputValid && (absensi < 1 || absensi > 16)" class="text-red-500">Absensi harus berada dalam rentang 1 hingga 16.</p>
          </div>
          <div class="mb-4">
            <label for="tugas" class="block text-gray-600 text-sm font-semibold mb-2">Tugas:</label>
            <input v-model="tugas" type="number" id="tugas" name="tugas" class="w-full px-3 py-2 border rounded-lg text-gray-700 focus:outline-none focus:border-blue-500" required>
            <p v-if="!isInputValid && (tugas < 1 || tugas > 100)" class="text-red-500">Tugas harus berada dalam rentang 1 hingga 100.</p>
          </div>
          <div class="mb-4">
            <label for="uts" class="block text-gray-600 text-sm font-semibold mb-2">UTS:</label>
            <input v-model="uts" type="number" id="uts" name="uts" class="w-full px-3 py-2 border rounded-lg text-gray-700 focus:outline-none focus:border-blue-500" required>
            <p v-if="!isInputValid && (uts < 1 || uts > 100)" class="text-red-500">UTS harus berada dalam rentang 1 hingga 100.</p>
          </div>
          <div class="mb-4">
            <label for="uas" class="block text-gray-600 text-sm font-semibold mb-2">UAS:</label>
            <input v-model="uas" type="number" id="uas" name="uas" class="w-full px-3 py-2 border rounded-lg text-gray-700 focus:outline-none focus:border-blue-500" required>
            <p v-if="!isInputValid && (uas < 1 || uas > 100)" class="text-red-500">UAS harus berada dalam rentang 1 hingga 100.</p>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="pb-12 mt-4">
    <div class="mx-auto max-w-2xl">
      <div class="overflow-hidden bg-white shadow-sm sm:rounded-lg">
        <div class="p-12 bg-white border-b border-gray-200">
          <h1>{{ nama }}</h1>
          <h1>Mendapatkan Nilai Akhir: {{ nilaiAkhir }}</h1>
          <h1>Mendapatkan Nilai Huruf: {{ nilaiHuruf }}</h1>
        </div>
      </div>
    </div>
  </div>
</template>
