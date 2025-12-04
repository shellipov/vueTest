<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
import { fromEvent, Subscription } from 'rxjs';

const count = ref(0);
const secondCount = ref(0);
const btn = ref<HTMLButtonElement | null>(null);

let subscription: Subscription | null = null;

onMounted(() => {
  if (!btn.value) return;

  const click$ = fromEvent(btn.value, 'click');

  subscription = click$.subscribe(() => {
    console.log('Кнопка нажата через RxJS!');
    secondCount.value += 1;
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
    <button ref="btn">
      rxJs - {{ secondCount }}
    </button>
  </div>
</template>

<style scoped>
.card {
  margin: 10px 0;
}
</style>
