<template>
  <div class="cell" @click="handleClick">
    <span
      class="symbol"
      :class="{ green: value === 'X', blue: value === 'O' }"
      >{{ value }}</span
    >
  </div>
</template>

<script setup lang="ts">
const props = defineProps({
  value: { type: String, default: "" },
});

const emit = defineEmits(["click"]);

const handleClick = () => {
  emit("click");
};
</script>

<style scoped>
.cell {
  width: 100px;
  height: 100px;
  border: 4px solid black;
  background-color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  font-size: 2em;
}

.symbol.green {
  color: green;
  font-size: 5rem;
}

.symbol.blue {
  color: blue;
  font-size: 5rem;
}
</style>
