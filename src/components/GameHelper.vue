<template>
  <div class="game-helper" v-if="playerName">
    It's your turn, <span class="name-color">{{ playerName }}</span>
  </div>
</template>

<script setup lang="ts">
const props = defineProps({
  playerName: String,
});
</script>

<style scoped>
.game-helper {
  font-size: 1.5rem;
  position: relative;
  bottom: 50px;
  color: whitesmoke;
}

.name-color {
  color: #b0daff;
  font-weight: 600;
  font-size: 2.5rem;
}
</style>
