<template>
  <div>
    <div  class="panel">
        <div v-if="timeRemaining.days >= 0">
            <h1>
                Осталось дней: {{ timeRemaining.days }}
            </h1>
            <h1>Осталось часов: {{ timeRemaining.hours }}</h1>
            <h1>Осталось минут: {{ timeRemaining.minutes }}</h1>
            <h1>Осталось секунд: {{ timeRemaining.seconds }}</h1>
        </div>
        <div v-else>
            <p>Сегодня домой!</p>
        </div>
    </div>
  </div>
</template>

<script>
export default {
    data() {
        return {
            countdownDate: new Date('2023-11-21T23:59:59'), // Здесь указываем дату окончания обратного отсчета
            timeRemaining: {
                days: 0,
                hours: 0,
                minutes: 0,
                seconds: 0
            },
            countdownInterval: null
        };
    },
    mounted() {
        // Вычисляем оставшееся время при монтировании компонента
        this.calculateCountdown();
        // Запускаем интервал для обновления оставшегося времени каждую секунду
        this.countdownInterval = setInterval(this.calculateCountdown, 1000);
    },
    beforeUnmount() {
        // Очищаем интервал перед размонтированием компонента
        clearInterval(this.countdownInterval);
    },
    methods: {
        calculateCountdown() {
            const currentTime = new Date();
            const timeDifference = this.countdownDate - currentTime;
            if (timeDifference > 0) {
                this.timeRemaining.days = Math.floor(timeDifference / (1000 * 60 * 60 * 24));
                this.timeRemaining.hours = Math.floor((timeDifference / (1000 * 60 * 60)) % 24);
                this.timeRemaining.minutes = Math.floor((timeDifference / (1000 * 60)) % 60);
                this.timeRemaining.seconds = Math.floor((timeDifference / 1000) % 60);
            } else {
                // Если время истекло, останавливаем интервал
                clearInterval(this.countdownInterval);
            }
        }
    }
};
</script>
<style>
body {
  background: url(https://images.wallpaperscraft.ru/image/single/fon_piatna_temnyj_91678_1920x1080.jpg);
  background-size: auto;
  height: 100%;
  background-repeat: repeat;
}
h1 {
  font-size: 24px;
}
.panel {
  margin-top: 100px;
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
