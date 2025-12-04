<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
import { fromEvent, Subscription } from 'rxjs';

const count = ref(0);
const secondCount = ref(0);
const btn = ref<HTMLButtonElement | null>(null);
const color = ref('#FF0000') // начальный цвет

function getRandomRgbColor() {
  const r = Math.floor(Math.random() * 256);
  const g = Math.floor(Math.random() * 256);
  const b = Math.floor(Math.random() * 256);
  return `rgb(${r}, ${g}, ${b})`;
}

let subscription: Subscription | null = null;

onMounted(() => {
  if (!btn.value) return;

  const click$ = fromEvent(btn.value, 'click');

  subscription = click$.subscribe(() => {
    console.log('Кнопка нажата через RxJS!');
    secondCount.value += 2;
    color.value = getRandomRgbColor()
  });
});

onUnmounted(() => {
  subscription?.unsubscribe();
});
</script>

<template>
  <div class="card">
    <button type="button" @click="count++">
      count is {{ count }}
    </button>
  </div>
  <div class="card">
    <button ref="btn" :style="{ backgroundColor: color }">
      rxJs - {{ secondCount }}
    </button>
  </div>
</template>

<style scoped>
.card {
  margin: 10px 0;
}
</style>
