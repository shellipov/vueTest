<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
import { fromEvent } from 'rxjs';

const count = ref(0)
const btn = ref(null);
const secondCount = ref(0);

let subscription = null;

onMounted(() => {
  // создаём Observable на событие клика
  const click$ = fromEvent(btn.value, 'click');

  // подписываемся на событие
  subscription = click$.subscribe(() => {
    console.log('Кнопка нажата!');

    secondCount.value += 1;
  });
});

onUnmounted(() => {
  // обязательно отписываемся
  subscription?.unsubscribe();
});

</script>

<template>
  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
  </div>
  <div class="card">
    <button ref="btn" >rxJs -  {{ secondCount }}</button>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
