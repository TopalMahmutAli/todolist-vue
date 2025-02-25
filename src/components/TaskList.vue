<template>
    <h3>To-Do-List</h3>
    <ul>
      <li 
        v-for="task in tasks" 
        :key="task.id" 
        :class="{ completed: task.completed }" 
        @click="toggleTask(task)"
      >
         {{ task.text }} 
        <button @click="$emit('remove-task', task.id)">Supprimer</button>
      </li>
      
    </ul>
  </template>
  
  <script setup>
 
  defineProps({
    tasks: Array,
    toggleTask: Function,
    removeTask: Function
  });
  </script>
  
  <style scoped>
  .completed {
    text-decoration: line-through;
    color: gray;
  }
  li {
  cursor: pointer;
  transition: 0.2s;
}

li:hover {
  color: blue;  
}

  </style>
  