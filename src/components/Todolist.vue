<template>
  <ul>
    <li v-for="(todo, index) in todos" :key="index" class="todo-item">
      <div v-if="editingIndex !== index" @dblclick="editTodo(index)">
        <span>{{ todo }}</span>
        <button @click="removeTodo(index)" class="done-button">Done</button>
      </div>
      <div v-else>
        <input type="text" v-model="todos[index]" @keyup.enter="stopEditing" @blur="stopEditing" />
      </div>
    </li>
  </ul>
</template>

<script setup>
import { ref } from 'vue'

// Props for the todo list
const props = defineProps({
  todos: Array
})

const emit = defineEmits(['remove-todo'])
const editingIndex = ref(null)

const editTodo = (index) => {
  editingIndex.value = index
}

const stopEditing = () => {
  editingIndex.value = null
}

const removeTodo = (index) => {
  emit('remove-todo', index)
}
</script>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
  font-family: 'Poppins', sans-serif;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #f5f5f5; 
  margin: 15px 0; /* Increased margin for more space between items */
  padding: 15px 20px; /* More padding for a better shape */
  border-radius: 12px; /* Rounded corners for a softer look */
  border: 1px solid #ddd;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); /* Slightly larger shadow */
  transition: box-shadow 0.2s ease, transform 0.2s ease; /* Added transform for hover effect */
}

.todo-item:hover {
  box-shadow: 0 6px 10px rgba(0, 0, 0, 0.2); /* Enhanced shadow on hover */
  transform: translateY(-2px); /* Lift effect on hover */
}

.todo-item span {
  flex-grow: 1;
  color: #333; 
  font-weight: 500; /* Bold text for better visibility */
}

.todo-item input {
  padding: 10px;
  width: 100%;
  font-size: 1rem;
  color: #333;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 4px;
  outline: none;
  transition: border-color 0.2s ease;
}

.todo-item input:focus {
  border-color: #007bff;
}

button.done-button {
  background-color: #23e36d;
  color: white;
  border: none;
  padding: 8px 12px; /* Adjusted padding for button */
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
  font-weight: 600; /* Bold text for better visibility */
}

button.done-button:hover {
  background-color: #ff3333;
}

button.done-button:focus,
input:focus {
  outline: none;
  box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.25);
}

body {
  background-color: #f0f0f0; 
  color: #333; 
}
</style>
