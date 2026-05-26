<script setup lang="ts">
import TodoItem from './TodoItem.vue'
import TodoFilter from './TodoFilter.vue'
import { ref, computed, watch } from 'vue'

const newTask = ref('')
const filter = ref('all')

interface Task {
  id: number
  text: string
  completed: boolean
}

const STORAGE_KEY = 'todo-tasks'

const savedTasks = localStorage.getItem(STORAGE_KEY)

const tasks = ref<Task[]>(
  savedTasks
    ? JSON.parse(savedTasks)
    : [
        {
          id: 1,
          text: '學 Vue.js',
          completed: false,
        },
        {
          id: 2,
          text: '完成 Todo App',
          completed: true,
        },
      ],
)

const filteredTasks = computed(() => {
  switch (filter.value) {
    case 'completed':
      return tasks.value.filter((task) => task.completed)

    case 'active':
      return tasks.value.filter((task) => !task.completed)

    default:
      return tasks.value
  }
})

const totalTasks = computed(() => {
  return tasks.value.length
})

const completedTasks = computed(() => {
  return tasks.value.filter((task) => task.completed).length
})

const activeTasks = computed(() => {
  return tasks.value.filter((task) => !task.completed).length
})

const addTask = () => {
  if (newTask.value.trim() === '') return

  tasks.value.push({
    id: Date.now(),
    text: newTask.value,
    completed: false,
  })

  newTask.value = ''
}

const toggleTask = (id: number) => {
  const task = tasks.value.find((task) => task.id === id)

  if (task) {
    task.completed = !task.completed
  }
}

const deleteTask = (id: number) => {
  tasks.value = tasks.value.filter((task) => task.id !== id)
}

watch(
  tasks,
  (newTasks) => {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(newTasks))
  },
  {
    deep: true,
  },
)
</script>

<template>
  <div class="app">
    <div class="todo-container">
      <h1>Todo App Pro</h1>
      <div class="stats">
        <div class="stat-card">
          <span class="number">
            {{ totalTasks }}
          </span>

          <span class="label"> 總任務 </span>
        </div>

        <div class="stat-card">
          <span class="number">
            {{ completedTasks }}
          </span>

          <span class="label"> 已完成 </span>
        </div>

        <div class="stat-card">
          <span class="number">
            {{ activeTasks }}
          </span>

          <span class="label"> 未完成 </span>
        </div>
      </div>

      <TodoFilter :currentFilter="filter" @change-filter="filter = $event" />

      <div class="input-group">
        <input v-model="newTask" type="text" placeholder="輸入你的任務..." />

        <button @click="addTask">新增</button>
      </div>

      <ul>
        <TodoItem
          v-for="task in filteredTasks"
          :key="task.id"
          :task="task"
          @toggle="toggleTask"
          @delete="deleteTask"
        />
      </ul>
    </div>
  </div>
</template>
