<script setup>
  import { ref, onMounted, onUnmounted } from 'vue';
  import confetti from 'canvas-confetti';
  import { 
    differenceInYears, 
    differenceInMonths, 
    differenceInDays,
    differenceInMinutes,
    differenceInSeconds
  } from 'date-fns';

const date = new Date();
const isBirthday = date.getDate() === 8 && date.getMonth() === 2;

const texts = ref([
  'Hello, dear Ingriin',
  `Today is ${date.toLocaleDateString('en-US', { year: 'numeric', month: 'short', day: 'numeric' })}`,
  'You know what that means?',
  isBirthday ? 'It is your birthday!' : 'It was your birthday!',
  'And I couldn\'t be more proud of you. You\'ve gone through so much, although so young. Yet all of it has just turned you into the beautiful soul you are today. You have so much joy to share with the world and everyone around you should consider themselves very lucky to have you in their lives. I for sure know how lucky I\'ve gotten to find you in this chaotic generation <3']);
const currentText = ref(0);

const cycleText = () => {
  currentText.value = (currentText.value + 1) % texts.value.length;
  if (currentText.value === 3 && isBirthday) {
    triggerConfetti();
  }
};

const triggerConfetti = () => {
  const colors = ['#01befe', '#ffdd00', '#ff7d00', '#ff006d', '#adff02', '#8f00ff'];
    confetti({
      particleCount: 25,
      angle: 75,
      spread: 45,
      origin: { x: 0, y: 0.8 },
      colors: colors,
      shapes: ['circle', 'square'],
      drift: 1
    });
    confetti({
      particleCount: 25,
      angle: 105,
      spread: 45,
      origin: { x: 1, y: 0.7 },
      colors: colors,
      shapes: ['circle', 'square'],
      drift: -1
    });
    confetti({
      particleCount: 25,
      angle: 85,
      spread: 60,
      origin: { x: 0, y: 0.55 },
      colors: colors,
      shapes: ['circle', 'square'],
      drift: 2
    });
    confetti({
      particleCount: 25,
      angle: 95,
      spread: 60,
      origin: { x: 1, y: 0.45 },
      colors: colors,
      shapes: ['circle', 'square'],
      drift: -2
    });
};

const years = ref('');
const months = ref('');
const days = ref('');
const minutes = ref('');
const seconds = ref('');
let intervalId;

const calculateAge = () => {
  const startDate = new Date('2004-03-08');
  const now = new Date();
  
  years.value = differenceInYears(now, startDate);
  months.value = differenceInMonths(now, startDate);
  days.value = differenceInDays(now, startDate);
  minutes.value = differenceInMinutes(now, startDate);
  seconds.value = differenceInSeconds(now, startDate);
};

onMounted(() => {
  calculateAge();
  intervalId = setInterval(calculateAge, 1000); // Update every second
});

onUnmounted(() => {
  if (intervalId) clearInterval(intervalId);
});
</script>

<template>
  <div class="background-heart"></div>
  <div class="text-container" :class="{ active: currentText === 0 }" @click="cycleText">
    {{ texts[0] }}
  </div>
  <div class="text-container" :class="{ active: currentText === 1 }" @click="cycleText">
    {{ texts[1] }}
  </div>
  <div class="text-container" :class="{ active: currentText === 2 }" @click="cycleText">
    {{ texts[2] }}
  </div>
  <div class="text-container" :class="{ active: currentText === 3 }" @click="cycleText">
    {{ texts[3] }}
  </div>
  <div class="text-container" :class="{ active: currentText === 4 }" @click="cycleText">
    {{ texts[4] }}
  </div>

  <div class="age-counter">
    {{ years }} years<br>
    {{ months }} months<br>
    {{ days }} days<br>
    {{ minutes }} minutes<br>
    {{ seconds }} seconds
  </div>
</template>

<style>
.background-heart {
  position: fixed;
  width: 50px;
  height: 50px;
  top: 10px;
  left: 5px;
  background-image: url('/src/assets/heart.svg');
  background-repeat: no-repeat;
  background-position: center;
  background-size: contain;
  opacity: 1;
  z-index: -1;
  filter: invert(100%);
  opacity: .6;
}

.text-container {
  width: 80vw;
  opacity: 0;
  transition: opacity 0.5s ease;
}

.active {
  opacity: 1;
}

.age-counter {
  position: fixed;
  line-height: 14px;
  text-align: end;
  bottom: 20px;
  right: 20px;
  font-size: 14px;
  color: rgba(255, 255, 255, 0.6);
  user-select: none;
}
</style>
