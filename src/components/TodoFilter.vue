<script setup lang="ts">
defineProps<{
  currentFilter: string
}>()

const emit = defineEmits(['change-filter'])
</script>

<template>
  <div class="filter-group">
    <button :class="{ active: currentFilter === 'all' }" @click="emit('change-filter', 'all')">
      全部
    </button>

    <button
      :class="{ active: currentFilter === 'active' }"
      @click="emit('change-filter', 'active')"
    >
      未完成
    </button>

    <button
      :class="{ active: currentFilter === 'completed' }"
      @click="emit('change-filter', 'completed')"
    >
      已完成
    </button>
  </div>
</template>

<style scoped>
.filter-group {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

button {
  flex: 1;
  padding: 10px;

  border: none;
  border-radius: 10px;

  background: #2a2a2a;
  color: white;

  cursor: pointer;
  transition: 0.3s;
}

button:hover {
  background: #3a3a3a;
}

.active {
  background: #646cff;
}
</style>
