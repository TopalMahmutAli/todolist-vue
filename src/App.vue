<template>
<h3>Ma To-Do List</h3>
<input v-model="newTaskText" 
  placeholder="Nouvelle tâche" 
  @keydown.enter="addTask" />
<button @click="addTask" type="button">Ajouter</button>
<div class="filter-container">
  <select v-model="filtre">
    <option value="all">Toutes</option>
    <option value="completed">Terminées</option>
    <option value="incomplete">Non terminées</option>
  </select>
  <p>{{ filteredTasks.length }} tâche(s) affichée(s)</p>
</div>
<TaskList :tasks="filteredTasks" :toggleTask="toggleTask" @removeTask="removeTask" @updateTask="updateLocalStorage" />

</template>


<script setup>
import TaskList from './components/TaskList.vue';
import { ref, onMounted, computed } from 'vue';

const tasks = ref([]);
const newTaskText = ref('');
const filtre = ref("all");

const updateLocalStorage = () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value));
};

onMounted(() => {
  try {
    const savedTasks = localStorage.getItem('tasks');
    tasks.value = savedTasks ? JSON.parse(savedTasks) : [];
  } catch (error) {
    tasks.value = [];
    console.error('Erreur lors du chargement des tâches depuis localStorage', error);
  }
});

const toggleTask = (task) => {
  task.completed = !task.completed;
  updateLocalStorage();
};

const addTask = () => {
  if (newTaskText.value.trim() !== '') {
    const newTask = {
      id: Date.now(),
      text: newTaskText.value,
      date: new Date().toLocaleString(),
      isEditing: false,
      completed: false
    };
    tasks.value.push(newTask);
    updateLocalStorage();
    newTaskText.value = '';
    console.log("Tâche ajoutée :", newTask);
  }
};

const removeTask = (taskId) => {
  tasks.value = tasks.value.filter(task => task.id !== taskId);
  updateLocalStorage();
};

const filteredTasks = computed(() => {
  if (filtre.value === "completed") {
    return tasks.value.filter(task => task.completed);
  } else if (filtre.value === "incomplete") {
    return tasks.value.filter(task => !task.completed);
  }
  return tasks.value;
});
</script>

<style scoped>
input {
  margin-right: 10px;
}
button {
  padding: 5px 10px;
}

select {
  padding: 8px 12px;
  font-size: 16px;
  border: 2px solid #007bff;
  border-radius: 5px;
  background-color: #fff;
  color: #333;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
}


select:hover, select:focus {
  border-color: #0056b3;
  box-shadow: 0 0 5px rgba(0, 91, 187, 0.5);
  outline: none;
}


p {
  font-size: 18px;
  color: #333;
  font-weight: bold;
  margin-top: 10px;
  background: #f8f9fa;
  padding: 8px;
  border-radius: 5px;
  display: inline-block;
  border: 1px solid #ddd;
}


</style>

