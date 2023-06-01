<template>
  <div v-if="winner" class="winner-announcement">
    <p v-if="winner === 'draw'">The game is a draw.</p>
    <p v-else>
      <span class="winner-name">{{ winner }}</span> is the winner!
    </p>
    <button @click="$emit('resetGame')" class="play-again-button">
      Play again
    </button>
  </div>
</template>

<script setup lang="ts">
const props = defineProps({
  winner: String,
});

defineEmits(["resetGame"]);
</script>

<style scoped>
.winner-announcement {
  font-size: 2rem;
  color: whitesmoke;
  margin-top: 50px;
}

.play-again-button {
  background-color: aquamarine;
  cursor: pointer;
  height: 38px;
  width: 160px;
  border: 3px solid rgb(20, 20, 20);
  font-size: 18px;
}

.winner-name {
  color: #b0daff;
  font-weight: 600;
  font-size: 3rem;
}
</style>
