<template>
  <div class="flex items-center justify-center h-screen">
    <div class="card bg-base-100 w-96 shadow-xl">
      <div class="card-body">
        <h2 class="card-title"> Vex Puan Hesaplama!</h2>
        <p>Ozan Kaygusuz</p>
        <h2 class="card-title">Puan => {{ sonuc }}</h2>
        <p class="font-bold">Açılan Pota => {{ p }}</p>
        <button class="btn btn-blue" @click="p1">+</button>
        <button class="btn btn-yellow" @click="p0">-</button>
        <p class="font-bold">Basket Sayısı => {{ b }}</p>
        <button class="btn btn-blue" @click="b1">+</button>
        <button class="btn btn-yellow" @click="b0">-</button>
        <button class="btn btn-primary mt-9" @click="calculatePoints">Hesapla</button>
      </div>
    </div>
  </div>
   <footer class="footer p-10 bg-base-300 text-base-content">
    <div>
      <h2 class="font-bold text-lg">Ozan Kaygusuz</h2>
      <p>Powered by modern web technologies:</p>
      <ul class="list-disc ml-6">
        <li>Vue.js</li>
        <li>Tailwind CSS</li>
        <li>DaisyUI</li>
      </ul>
      <p class="mt-2">© 2024 All rights reserved.</p>
    </div>
  </footer>
</template>

<script setup>
import { ref } from 'vue';

const p = ref(0);
const b = ref(0);
const sonuc = ref('');

// Increment and decrement functions for "Açılan Pota"
const p1 = () => {
  p.value++;
};

const p0 = () => {
  if (p.value > 0) {
    p.value--;
  }
};

// Increment and decrement functions for "Basket Sayısı"
const b1 = () => {
  b.value++;
};

const b0 = () => {
  if (b.value > 0) {
    b.value--;
  }
};

// Calculate points based on "Açılan Pota" and "Basket Sayısı"
const calculatePoints = () => {
  let pointsPerGoal = 0;

  // Determine points per goal based on the number of "Açılan Pota"
  switch (p.value) {
    case 1:
      pointsPerGoal = 4;
      break;
    case 2:
      pointsPerGoal = 8;
      break;
    case 3:
      pointsPerGoal = 10;
      break;
    case 4:
      pointsPerGoal = 12;
      break;
    default:
      pointsPerGoal = 0;
  }

  // Calculate total points
  const switchPoints = p.value * 1;
  const goalPoints = b.value * pointsPerGoal;
  const totalPoints = switchPoints + goalPoints;

  // Update the result
  sonuc.value = `${totalPoints}`;
};
</script>

<style>
html,
body {
  margin: 0;
  height: 100%;
}
</style>
