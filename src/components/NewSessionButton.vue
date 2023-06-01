<template>
  <button class="new-session" @click="$emit('newSession')">New Session</button>
</template>

<script setup lang="ts">
defineEmits(["newSession"]);
</script>

<style scoped>
.new-session {
  background-color: aquamarine;
  cursor: pointer;
  height: 38px;
  width: 160px;
  border: 3px solid rgb(20, 20, 20);
  font-size: 18px;
  margin-top: 90px;
}
</style>
