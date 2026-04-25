<script setup>
import Dice from './components/Dice.vue';


</script>

<template>
  <header>
    <h1>Dadu Digital</h1>
  </header>

  <main>
    <Dice />
  </main>
</template>

<style scoped>
header {
  text-align: center;
  padding: 20px;
}

main {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 50vh;
}
</style>
