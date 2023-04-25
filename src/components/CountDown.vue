<template>
  <div>
    <div  class="panel">
        <div class="block" v-if="timeRemaining.days >= 0">
            <div>
                <h1>Осталось</h1>
                <div class="time">
                    <h2>Дней: {{ timeRemaining.days }}</h2>
                    <h2 :class="{none: isOnlyDays, all: isAll}" >Часов: {{ timeRemaining.hours }}</h2>
                    <h2 :class="{none: isOnlyDays, all: isAll}" >Минут: {{ timeRemaining.minutes }}</h2>
                    <h2 :class="{none: isOnlyDays, all: isAll}" >Секунд: {{ timeRemaining.seconds }}</h2>
                </div>
            </div>
            <div>
                <h1>Прошло</h1>
                <div class="time">
                    <h2>Дней: {{ elapsedTime.days }}</h2>
                    <h2 :class="{none: isOnlyDays, all: isAll}">Часов: {{ elapsedTime.hours }}</h2>
                    <h2 :class="{none: isOnlyDays, all: isAll}">Минут: {{ elapsedTime.minutes }}</h2>
                </div>
            </div>

        </div>
        <div v-else>
            <h1>Сегодня домой!</h1>
        </div>
        <button @click="onlyDays">Показывать только дни</button>
        <button @click="all">Показывать все</button>
    </div>
  </div>
</template>

<script>
export default {
    data() {
        return {
            isOnlyDays: false,
            isAll: true,
            countdownDate: new Date('2023-11-21T23:59:59'), // Здесь указываем дату окончания обратного отсчета
            timeRemaining: {
                days: 0,
                hours: 0,
                minutes: 0,
                seconds: 0
            },
            countdownInterval: null,
            startDate: new Date('2022-11-21T08:00:00'), // Здесь указываем начальную дату
            elapsedTime: {
                days: 0,
                hours: 0,
                minutes: 0,
            },
            elapsedInterval: null,
        };
    },
    mounted() {
        // Вычисляем оставшееся время при монтировании компонента
        this.calculateCountdown();
        // Запускаем интервал для обновления оставшегося времени каждую секунду
        this.countdownInterval = setInterval(this.calculateCountdown, 1000);
        // Вычисляем прошедшее время при монтировании компонента
        this.calculateElapsedTime();
        // Запускаем интервал для обновления прошедшего времени каждую секунду
        this.elapsedInterval = setInterval(this.calculateElapsedTime, 1000);
    },
    beforeUnmount() {
        // Очищаем интервал перед размонтированием компонента
        clearInterval(this.countdownInterval);
        // Очищаем интервал перед размонтированием компонента
        clearInterval(this.elapsedInterval);
    },
    methods: {
        onlyDays() {
          this.isOnlyDays = true
            this.isAll = false
        },
        all() {
          this.isAll = true
            this.isOnlyDays = false
        },
        calculateElapsedTime() {
            const currentTime = new Date();
            const timeDifference = currentTime - this.startDate;
            this.elapsedTime.days = Math.floor(timeDifference / (1000 * 60 * 60 * 24));
            this.elapsedTime.hours = Math.floor((timeDifference / (1000 * 60 * 60)) % 24);
            this.elapsedTime.minutes = Math.floor((timeDifference / (1000 * 60)) % 60);
        },
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
    background-size: auto;
    margin: 10px;
    height: 100%;
    background: url(https://images.wallpaperscraft.ru/image/single/fon_piatna_temnyj_91678_1920x1080.jpg) repeat;
}
.block {
    margin-bottom: 20px;
    display: flex;
    flex-direction: column;
    gap: 20px;
}
.time {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    justify-content: center;
}
h2 {
    margin: 0;
    font-size: 20px;
}
.all {
    display: block;
}
.none {
    display: none;
}
button {
    padding: 10px;
    border: 1px solid white;
    background: none;
    color: white;
    border-radius:20px;
}
h1 {
  font-size: 24px;
    margin-bottom: 30px;
}
.panel {
  border: 1px solid white;
  max-width: 600px;
    margin: 100px auto auto;
    color: white;
  padding: 20px;
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.2);

  backdrop-filter: blur(8px);
}
</style>
