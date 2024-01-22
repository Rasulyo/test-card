<template>
  <div class="card-container">
    <div
      v-for="(flipped, index) in isFlipped"
      :key="index"
      class="card"
      :class="{ flipped: flipped, lifting: isAnimating && index === liftingIndex }"
      @click="flipCard(index)"
    >
      <div class="card-face front" v-if="!flipped"></div>
      <div class="card-face back" v-if="flipped"></div>
    </div>
    <button @click="startAnimation">Запустить анимацию</button>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const isFlipped = ref([false, false, false]);
const isAnimating = ref(false);
const liftingIndex = ref<number | null>(null);

const flipCard = (index: number) => {
  if (!isAnimating.value && index !== liftingIndex.value) {
    isFlipped.value[index] = !isFlipped.value[index];
  }
};

const startAnimation = () => {
  if (!isAnimating.value) {
    isAnimating.value = true;

    liftingIndex.value = 1;

    setTimeout(() => {
      isAnimating.value = false;
      liftingIndex.value = null;
    }, 700);
  }
};
</script>

<style scoped>
.card-container {
  display: flex;
  justify-content: space-around;
  align-items: center;
  height: 40vh;
  perspective: 1000px;
}

.card {
  width: 100px;
  height: 150px;
  position: relative;
  cursor: pointer;
  transform-style: preserve-3d;
  transition: transform 0.7s ease-in-out;
}

.card-face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  left: 0px;
}

.front {
  background: url('../assets/back.jpg') center center no-repeat;
  background-size: contain;
}

.back {
  background: url('../assets/front.jpg') center center no-repeat;
  transform: rotateY(180deg);
  background-size: contain;
}

.flipped {
  transform: rotateY(180deg);
}

.lifting {
  animation: liftUp 0.7s ease-in-out;
}

@keyframes liftUp {
  0% {
    transform: translateY(0);
  }
  100% {
    transform: translateY(-100px);
  }
}

button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}
</style>
