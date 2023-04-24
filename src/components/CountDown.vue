<template>
  <div>
    <div class="panel">
      <h1>
        {{ daysRemaining }} дней осталось до {{ day }}.{{ month }}.{{ year }}!
      </h1>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      year: 2023,
      month: 11,
      day: 23,
      daysRemaining: 0,
    };
  },
  mounted() {
    // Calculate the remaining days when the component is mounted
    this.calculateDaysRemaining();
  },
  methods: {
    calculateDaysRemaining() {
      const today = new Date();
      const targetDate = new Date(this.year, this.month - 1, this.day);
      const difference = targetDate - today;
      this.daysRemaining = Math.ceil(difference / (1000 * 60 * 60 * 24));
    },
  },
};
</script>
<style>
body {
  background: url(https://images.wallpaperscraft.ru/image/single/fon_piatna_temnyj_91678_1920x1080.jpg);
  background-size: cover;
}
.panel {
  border: 1px solid white;
  max-width: 600px;
  margin: auto;
  color: white;
  padding: 50px;
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(8px);
}
</style>
