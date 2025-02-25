<template>
<input v-model="newTaskText" placeholder="Nouvelle tâche" />
<button @click="addTask" type="button">Ajouter</button>
<TaskList :tasks="tasks" :toggleTask="toggleTask" @removeTask="removeTask" />
</template>

<script setup>
import TaskList from './components/TaskList.vue';
import { ref, onMounted } from 'vue';

const tasks = ref([]);
const newTaskText = ref('');

const updateLocalStorage = () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value));
};

onMounted(() => {
  
  try {
    const savedTasks = localStorage.getItem('tasks');
    
    if (savedTasks) {
      tasks.value = JSON.parse(savedTasks);
    } else {
      tasks.value = [];
    }
  } catch (error) {
    // Si une erreur se produit (par exemple, une erreur de parsing), on initialise une liste vide
    tasks.value = [];
    console.error('Erreur lors du chargement des tâches depuis localStorage', error);
  }
});

const toggleTask = (task) =>{
  task.completed = !task.completed;
  updateLocalStorage();
};



const addTask = ()=>{
  if (newTaskText.value.trim() !== ''){
    const newTask = {
      id: Date.now(),
      text: newTaskText.value,
      completed: false
    };

    tasks.value.push(newTask);
    updateLocalStorage();
    newTaskText.value = '';
  }
};

const removeTask = (taskId) =>{
  tasks.value = tasks.value.filter(task => task.id !== taskId);
  updateLocalStorage();
};



</script>




<style scoped>
input {
  margin-right: 10px;
}
button {
  padding: 5px 10px;
}
</style>

