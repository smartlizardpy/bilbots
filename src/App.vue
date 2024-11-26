<template>
  <div class="flex items-center justify-center h-screen">
    <div class="card bg-base-100 w-96 shadow-xl">
      <div class="card-body">
        <h2 class="card-title">Vex Puan Hesaplama!</h2>
        <p>Ozan Kaygusuz</p>
        <h2 class="card-title">Puan => {{ sonuc }}</h2>
        <p class="font-bold">Açılan Pota => {{ p }}</p>
        <button class="btn btn-blue" @click="p1">+</button>
        <button class="btn btn-yellow" @click="p0">-</button>
        <p class="font-bold">Basket Sayısı => {{ b }}</p>
        <button class="btn btn-blue" @click="b1">+</button>
        <button class="btn btn-yellow" @click="b0">-</button>
        <select class="select select-bordered w-full max-w-xs" v-model="isim">
          <option disabled selected>İsim</option>
          <option>Ahmet Çınar</option>
          <option>Alperen</option>
          <option>Atakan</option>
          <option>Doruk</option>
          <option>Egemen</option>
          <option>Erkut</option>
          <option>Ozan</option>
        </select>
        <button class="btn btn-primary mt-9" @click="calculatePoints">Hesapla</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const p = ref(0);
const b = ref(0);
const sonuc = ref("");
const isim = ref("");
const leaderboard = ref([]);

// Increment and decrement functions for "Açılan Pota"
const p1 = () => {
  p.value++;
};

const p0 = () => {
  if (p.value > 0) p.value--;
};

// Increment and decrement functions for "Basket Sayısı"
const b1 = () => {
  b.value++;
};

const b0 = () => {
  if (b.value > 0) b.value--;
};

// Fetch leaderboard data from the API
const fetchLeaderboard = async () => {
  try {
    const response = await fetch(
      "https://script.google.com/macros/s/AKfycbz9h9rKM0MjPHsUf9eYZ_Wg3yAh4qOiCwx-G33uqh9ejznpXc9oIkVngKJNyrf8nxPAEw/exec"
    );
    if (response.ok) {
      leaderboard.value = await response.json();
    } else {
      console.error("API bağlantısı başarısız oldu.");
    }
  } catch (error) {
    console.error("Leaderboard alınırken hata oluştu:", error);
  }
};

// Calculate points based on "Açılan Pota" and "Basket Sayısı"
const calculatePoints = async () => {
  if (!isim.value) {
    alert("Lütfen bir isim seçin.");
    return;
  }

  try {
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

    // Send points to API
    const response = await fetch(
      "https://script.google.com/macros/s/AKfycbz9h9rKM0MjPHsUf9eYZ_Wg3yAh4qOiCwx-G33uqh9ejznpXc9oIkVngKJNyrf8nxPAEw/exec",
      {
        method: "POST",
        body: JSON.stringify({ name: isim.value, points: totalPoints }),
      }
    );
    if (!response.ok) {
      console.error("API'ye puan gönderimi başarısız oldu.");
    } else {
      console.log("Puan başarıyla gönderildi.");
      await fetchLeaderboard(); // Refresh leaderboard after submitting points
    }
  } catch (error) {
    console.error("Puan gönderirken hata oldu:", error);
  }
  location.reload();
};

// Fetch the leaderboard data on component mount
onMounted(() => {
  fetchLeaderboard();
});
</script>

<style>
html,
body {
  margin: 0;
  height: 100%;
}
</style>
