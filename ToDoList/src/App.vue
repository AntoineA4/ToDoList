<template>
  <h1>Your own to do list</h1>
  <hr>
  <div v-show="tasks.length === 0">No task plan yet</div>
  <label>
    <input type="checkbox"
    v-model="hideCompleted">
    Hide completed tasks
  </label>
  <form action="" @submit.prevent="addTask">
    <input type="text" placeholder="New Task" v-model="newTask">
    <button :disabled="newTask.length === 0">Add task</button>
  </form>
  <ul :style="{listStyle: 'none'}">
    <li v-for="task in sortedTasks" 
    :key="task.date" 
    :class="{ done: task.completed }">
      <input type="checkbox" 
      v-model="task.completed"
      />
      {{task.text}}
    </li>
  </ul>

</template>

<script setup>
import {ref, computed} from 'vue'

const tasks = ref ([
])
const newTask = ref ('')
const addTask = () => { 
  tasks.value.push({text: newTask.value, completed: false, date: Date.now()})
  newTask.value = ''
}
const sortedTasks = computed(() => {
  const sortedTasks = tasks.value.sort((a, b) => a.completed - b.completed) 
  if (hideCompleted.value === true) {
    return sortedTasks.filter(t => t.completed === false)
  }
  return sortedTasks
})
const hideCompleted = ref(false)
</script>

<style>
.done {
  text-decoration: line-through;
  }
</style>