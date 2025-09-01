<template>
  <div class="cell" @click="onClick">
    {{ value }}
  </div>
</template>

<script setup>
const props = defineProps({
  value: {
    type: String,
    default: "",
  },
});

const emit = defineEmits(["cell-click"]);

const onClick = () => {
  emit("cell-click");
};
</script>

<style scoped>
.cell {
  width: 80px;
  height: 80px;
  border: 2px solid #2c3e50;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2.5rem;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.2s;
}

.cell:hover {
  background-color: #f0f0f0;
}
</style>
