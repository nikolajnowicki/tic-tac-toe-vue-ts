<template>
  <div class="scoreboard">
    {{ playerName }}
    <div class="wins">
      Wins: <span class="win-number">{{ wins }}</span>
    </div>
  </div>
</template>

<script setup lang="ts">
const props = defineProps({
  playerName: String,
  wins: Number,
  playerSide: String,
});
</script>

<style scoped>
.scoreboard {
  display: flex;
  flex-direction: column;
  position: relative;
  margin-left: 120px;
  margin-right: 120px;
  color: #b0daff;
  font-size: 2rem;
  top: 70px;
}

.wins {
  font-size: 1.2rem;
  display: block;
  color: aquamarine;
  margin-top: 24px;
}

.win-number {
  font-size: 1.5rem;
  color: aquamarine;
}
</style>
