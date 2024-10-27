<template>
  <div class="input-container">
    <input
      type="text"
      v-model="newTodo"
      @keyup.enter="addTodo"
      placeholder="Add a task"
      class="todo-input"
    />
    <button @click="addTodo" class="add-button">Add</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const newTodo = ref('')
const emit = defineEmits(['add-todo'])

const addTodo = () => {
  if (newTodo.value.trim() === '') return
  emit('add-todo', newTodo.value)
  newTodo.value = ''
}
</script>

<style scoped>
/* Container for input and button */
.input-container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 2rem;
  margin-left: 2rem;
}

/* Styling for the input */
.todo-input {
  padding: 0.5rem;
  border: 2px solid #ddd;
  border-radius: 4px;
  margin-right: 0.5rem;
  color: #09ca90; /* Darker text color */
  font-family: 'poppins', sans-serif;
  font-size: 1rem;
  width: 250px; /* Sets a fixed width for the input */
  transition: border-color 0.2s ease;
}

.todo-input:focus {
  outline: none;
  border-color: #09ca90; /* Blue border on focus */
}

/* Styling for the Add button */
.add-button {
  padding: 0.5rem 1rem;
  background-color: #acff4d; /* Light red background */
  color: rgb(137, 120, 158);
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-family: 'poppins', sans-serif;
  font-size: 1rem;
  font-weight: 600;
  transition: background-color 0.3s;
}

.add-button:hover {
  background-color: #afaebc; /* Darker red on hover */
}

.add-button:focus {
  outline: none;
  box-shadow: 0 0 0 3px rgba(255, 77, 77, 0.25); /* Red focus outline */
}
</style>
