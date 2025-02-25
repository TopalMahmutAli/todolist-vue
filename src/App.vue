<template>
  <h3>To-Do-List</h3>
  <ul>
    <li v-for="task in tasks" 
      :key="task.id" 
      :class="{completed: task.completed}" 
      @click="toggleTask(task)">
        <strong>ID :</strong> {{ task.id }} - 
        <strong>Text :</strong> {{ task.text }} 
    </li>
  </ul>
</template>

<script setup>
import { ref } from 'vue';

// Déclarer la liste des tâches comme une variable réactive
const tasks = ref([
  { id: 1, text: "Acheter du lait", completed: false },
  { id: 2, text: "Coder la todolist", completed: true },
  { id: 3, text: "Faire du sport", completed: false },
  { id: 4, text: "Prendre une douche", completed: false },
  { id: 5, text: "Jouer à LOL", completed: true },
  { id: 6, text: "Regarder Game of Thrones", completed: false },
  { id: 7, text: "Lire le dernier chapitre de One Piece", completed: false }
]);
const toggleTask = (task) =>{task.completed = !task.completed;};  
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
