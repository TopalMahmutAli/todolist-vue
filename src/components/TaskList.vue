<template>
    <h3>To-Do-List</h3>
    <ul>
      <li 
        v-for="task in tasks" 
        :key="task.id" 
        :class="{ completed: task.completed }" 
        @click="toggleTask(task)"
      >
        <strong>ID :</strong> {{ task.id }} - 
        <strong>Text :</strong> {{ task.text }} 
      </li>
    </ul>
  </template>
  
  <script setup>
 
  defineProps({
    tasks: Array,
    toggleTask: Function
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
  color: blue;  /* Change la couleur au survol pour plus de feedback */
}

  </style>
  