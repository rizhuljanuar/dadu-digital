<script setup>
import { ref } from 'vue';

const diceValues = ref([1, 1]);
const isRolling = ref(false);
const lastRolledTime = ref(null);
const rollCount = ref(0);

const getDiceEmoji = (value) => {
  const emojiMap = {
    1: '⚀',
    2: '⚁',
    3: '⚂',
    4: '⚃',
    5: '⚄',
    6: '⚅'
  }

  // Kembalikan emoji berdasarkan nilai, atau tanda tanya jika tidak valid
  return emojiMap[value] || '❓'
}

const rollAllDice = () => {
  isRolling.value = true;
  rollCount.value++

  lastRolledTime.value = new Date().toLocaleTimeString();

  diceValues.value = diceValues.value.map(() => {
    return Math.floor(Math.random() * 6) + 1;
  });

  setTimeout(() => {
    isRolling.value = false;
  }, 500);
}

const addRice = () => {
  if (diceValues.value.length < 10) {
    diceValues.value.push(1);
  }
}

const removeDice = () => {
  if (diceValues.value.length > 1) {
    diceValues.value.pop();
  }
}

const resetDice = () => {
  diceValues.value = diceValues.value.map(() => 1);
  rollCount.value = 0;
  lastRolledTime.value = null;
}

const getTotal = () => {
  return diceValues.value.reduce((sum, val) => sum + val, 0);
}

const handleKeydown = (event) => {
  if (event.key === '' || event.key === 'Enter') {
    event.preventDefault();
    rollAllDice();
  }

  if (event.key === 'Escape') {
    resetDice();
  }
}

const hoveredDice = ref(null);

const onMouseEnter = (index) => {
  hoveredDice.value = index;
}

const onMouseLeave = () => {
  hoveredDice.value = null;
}
</script>

<template>
  <div 
    class="dice-container"
    @keydown="handleKeydown"
    tabindex="0"
  >
    <h2>Dadu Digital</h2>
    <p class="hint">Tekan <kbd>Space</kbd> atau <kbd>Enter</kbd> unutk roll dadu</p>

    <div class="dice-list">
      <div
        v-for="(diceValue, index) in diceValues"
        :key="index"
        class="dice"
        :class="{ hovered: hoveredDice === index }"
        @mouseenter="onMouseEnter(index)"
        @mouseleave="onMouseLeave"
        @click="() => { diceValues[index] = Math.floor(Math.random() * 6) + 1 }"
        :title="`Klik untuk roll dadu ${index + 1}`"
      >
        <div class="dice-face" :class="{ rolling: isRolling }">
          <span class="dot">{{ getDiceEmoji(diceValue) }}</span>
        </div>
        <p class="dice-number">{{ diceValue }}</p>
      </div>
    </div>

    <div class="stats">
      <div v-if="!isRolling" class="stat-box">
        <h3>Total: {{ getTotal() }}</h3>
      </div>

      <div v-else class="stat-box">
        <h3>Rollling...</h3>
      </div>

      <div class="stat-box">
        <p>Total Roll: {{ rollCount }}</p>
        <p v-if="lastRolledTime">Last Roll: {{ lastRolledTime }}</p>
      </div>
    </div>

    <div class="controls">
      <button @click="rollAllDice" class="btn-primary">
        Roll Semua!
      </button>

      <button 
        @click="addRice" 
        :disabled="diceValues.length >= 10"
        class="btn-secondary"
      >
        Tambah Dadu (Max 10)
      </button>

      <button
        @click="removeDice"
        :disabled="diceValues.length === 1"
        class="btn-secondary"
      >
        Kurangi Dadu (Min 1)
      </button>

      <button @click="resetDice" class="btn-danger">
        Reset Semua
      </button>
    </div>
  </div>
</template>

<style scoped>
.dice-container {
  text-align: center;
  padding: 30px;
  max-width: 600px;
  margin: 0 auto;
  outline: none;
}

.hint {
  color: #666;
  font-size: 14px;
  margin-bottom: 20px;
}

kbd {
  background: #f5f5f5;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 2px 6px;
  font-family: monospace;
  font-size: 12px;
}

.dice-list {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin: 30px 0;
  flex-wrap: wrap;
}

.dice {
  display: inline-block;
  cursor: pointer;
  transition: transform 0.2s;
}

.dice.hovered {
  transform: scale(1.1);
}

.dice-face {
  width: 100px;
  height: 100px;
  background: white;
  border: 3px solid #333;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 50px;
  box-shadow: 4px 4px 10px rgba(0, 0, 0, 0.2);
  transition: all 0.3s;
}

.dice-face.rolling {
  animation: shake 0.5s;
}

@keyframes shake {
  0%, 100% { transform: rotate(0deg); }
  25% { transform: rotate(-15deg); }
  75% { transform: rotate(15deg); }
}

.stats {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin: 20px 0;
  flex-wrap: wrap;
}

.stat-box {
  padding: 15px 20px;
  background: #f5f5f5;
  border-radius: 8px;
  min-width: 120px;
}

.stat-box h3 {
  margin: 0;
  color: #333;
}

.stat-box p {
  margin: 5px 0 0;
  font-size: 14px;
  color: #666;
}

.controls {
  display: flex;
  gap: 10px;
  justify-content: center;
  flex-wrap: wrap;
  margin-top: 20px;
}

button {
  padding: 12px 20px;
  font-size: 14px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s;
  font-weight: 500;
}

.btn-primary {
  background: #4CAF50;
  color: white;
}

.btn-primary:hover:not(:disabled) {
  background: #45a049;
  transform: scale(1.05);
}

.btn-secondary {
  background: #2196F3;
  color: white;
}

.btn-secondary:hover:not(:disabled) {
  background: #0b7dda;
}

.btn-danger {
  background: #f44336;
  color: white;
}

.btn-danger:hover {
  background: #da190b;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  transform: none !important;
}
</style>
