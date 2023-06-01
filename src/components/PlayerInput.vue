<template>
  <form @submit.prevent="handleSubmit">
    <input
      type="text"
      v-model="name"
      placeholder="Player Name"
      class="player-input"
    />
    <button type="submit" class="submit-button">Submit</button>
  </form>
</template>

<script setup lang="ts">
import { ref } from "vue";

const name = ref("");
const emit = defineEmits(["submitName"]);

const handleSubmit = () => {
  emit("submitName", name.value);
  name.value = "";
};
</script>

<style scoped>
.player-input {
  border: none;
  outline: none;
  background: none;
  border-bottom: 2px solid #b0daff;
  margin-right: 10px;
  color: #b0daff;
  font-size: 1.5rem;
  width: 180px;
}

.player-input::placeholder {
  color: #ffffff9f;
}

.submit-button {
  position: relative;
  bottom: 2px;
  background-color: aquamarine;
  cursor: pointer;
  height: 38px;
  width: 80px;
  border: 3px solid rgb(20, 20, 20);
  font-size: 18px;
}
</style>
