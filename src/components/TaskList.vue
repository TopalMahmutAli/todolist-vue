<template>
  <ul>
    <li 
      v-for="task in tasks" 
      :key="task.id" 
      :class="{ completed: task.completed }"
      @click="toggleTask(task)"
    >
      <span v-if="!task.isEditing">{{ task.text }}</span>

      <input 
        v-else 
        type="text" 
        v-model="task.text"
        @blur="saveTask(task)"
        @keyup.enter="saveTask(task)" 
        @keyup.esc="cancelEdit(task)"
        autofocus 
      />

      <!-- Conteneur des boutons pour les aligner -->
      <div class="button-group">
        <button @click="editTask(task)" v-if="!task.isEditing">✏️ Modifier</button>
        <button @click="saveTask(task)" v-else>✅ Sauvegarder</button>
        <button @click="$emit('removeTask', task.id)">🗑️ Supprimer</button>
      </div>
    </li>
  </ul>
</template>

  
<script setup>
defineProps({
  tasks: Array,
  toggleTask: Function,
  removeTask: Function
});



const editTask = (task) => {
  task.originalText = task.text;
  task.isEditing = true;
};

const emit = defineEmits(['removeTask', 'updateTask']);

const saveTask = (task) => {
  if (task.text.trim() === '') {
    task.text = 'Tâche sans titre'; 
  }
  task.isEditing = false;

  emit('updateTask', task); 
};

const cancelEdit = (task) => {
  task.text = task.originalText;
  task.isEditing = false;
}



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
button {
  margin-left: 10px;
  padding: 5px;
  cursor: pointer;
  border: none;
  border-radius: 5px;
  background-color: #007bff;
  color: white;
  transition: 0.2s;
}

button:hover {
  background-color: #0056b3;
}
</style>


  