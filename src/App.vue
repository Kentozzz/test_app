<template>
  <div id="app">
    <div>
      <label for="timeInput">カウントダウン時間（分）:</label>
      <input id="timeInput" type="number" v-model.number="inputMinutes" step="1" />

    </div>
    <div class="timer">{{ formatTime }}</div>
    <div class="controls">
      <button @click="setTime" :disabled="timerId !== null">セット</button>
      <button @click="startTimer" :disabled="timerId !== null || !timeSet">スタート</button>
      <button @click="stopTimer" :disabled="timerId === null">ストップ</button>
      <button @click="resetTimer">リセット</button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const inputMinutes = ref(0);
const time = ref(0);
let timerId = ref(null);
const timeSet = ref(false);
const startingTime = ref(0);

const formatTime = computed(() => {
  const minutes = Math.floor(time.value / 60);
  const seconds = ((time.value / 60) % 1) * 60;
  return `${minutes}:${seconds.toFixed(0).padStart(2, '0')}`;
});

function setTime() {
  startingTime.value = inputMinutes.value * 60; // 入力された時間（分）を秒数に変換
  time.value = startingTime.value; // 残り時間（秒数）をセット
  timeSet.value = true;
}

function startTimer() {
  timerId.value = setInterval(() => {
      if (time.value > 0) {
        time.value -= 1; // 1秒減らす
        // タイマーが0になった時の処理
        if (time.value === 0) {
          setTimeout(() => {
            alert("時間になりました!");
          }, 10); 
          resetTimer(); // タイマーをリセット
        }
      }
  }, 1000);
}

function stopTimer() {
  clearInterval(timerId.value);
  timerId.value = null;
}

function resetTimer() {
  clearInterval(timerId.value);
  timerId.value = null;
  time.value = startingTime.value;
}







</script>


<style>
html, body {
  margin: 0;
}

#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f5f5f5;
  color: #333;
}

.timer {
  font-size: 2.5em;
  margin: 20px 0;
  padding: 10px 20px;
  background-color: #fff;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  color: #4caf50;
}

div > div {
  margin-bottom: 15px;
}

label {
  margin-right: 10px;
}

input[type="number"] {
  font-size: 1em;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 3px;
  width: 80px; 
  box-sizing: border-box;
}

.controls {
  display: flex;
  gap: 10px;
}

button {
  cursor: pointer;
  font-size: 1em;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  background-color: #2196f3;
  color: white;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #1976d2;
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
</style>