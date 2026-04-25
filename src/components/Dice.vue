<script setup>
import { ref } from 'vue';

const diceValue = ref(1);

const rollDice = () => {
  const randomNum = Math.floor(Math.random() * 6) + 1;

  diceValue.value = randomNum;
}

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
  return emojiMap[value.value] || '❓'
}
</script>

<template>
  <div class="dice">
    <div class="dice-face">
      <span class="dot">{{ getDiceEmoji(diceValue) }}</span>
    </div>

    <button @click="rollDice" class="roll-btn">
      Roll Dadu!
    </button>

    <p class="dice-label">Angka: {{ diceValue }}</p>
  </div>
</template>

<style scoped>
.dice {
  display: inline-block;
  padding: 20px;
  text-align: center;
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
  margin: 0 auto 20px;
}

.roll-btn {
  padding: 12px 24px;
  font-size: 16px;
  background: #4CAF50;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.3s;
}

.roll-btn:hover {
  background: #45a049;
}

.roll-btn:active {
  transform: scale(0.95);
}

.dice-label {
  margin-top: 15px;
  font-weight: bold;
  font-size: 18px;
}
</style>
