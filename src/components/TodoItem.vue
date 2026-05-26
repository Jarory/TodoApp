<script setup lang="ts">
interface Task {
  id: number
  text: string
  completed: boolean
}

defineProps<{
  task: Task
}>()

const emit = defineEmits(['toggle', 'delete'])
</script>

<template>
  <li :class="{ completed: task.completed }">
    <div class="task-left">
      <input type="checkbox" :checked="task.completed" @change="emit('toggle', task.id)" />

      <span>
        {{ task.text }}
      </span>
    </div>

    <button @click="emit('delete', task.id)">刪除</button>
  </li>
</template>

<style scoped>
li {
  display: flex;
  justify-content: space-between;
  align-items: center;

  background: #1f1f1f;
  padding: 16px;
  border-radius: 12px;
  margin-bottom: 12px;

  transition: 0.3s;
}

li:hover {
  transform: translateY(-2px);
}

.task-left {
  display: flex;
  align-items: center;
  gap: 10px;
}

span {
  color: white;
}

.completed span {
  text-decoration: line-through;
  opacity: 0.5;
}

button {
  border: none;
  padding: 8px 12px;
  border-radius: 8px;
  cursor: pointer;

  background: #ff4d4f;
  color: white;
}
</style>
