<template>
  <div class="app">
    <h1 class="header">To Do App</h1>
    <TodoInput @add-todo="addTodo" />

    <!-- Display the number of completed tasks -->
    <p>Completed tasks: {{ completedTodosCount }} / {{ todos.length }}</p>

    <transition-group name="fade" tag="ul" class="todo-list">
      <li v-for="(todo, index) in todos" :key="index" :class="{'todo-done': todo.done}" class="todo-item">
        <div v-if="editingIndex !== index" @dblclick="editTodo(index)">
          <span>{{ todo.text }}</span>
        </div>
        <div v-else>
          <input type="text" v-model="todos[index].text" @keyup.enter="stopEditing" @blur="stopEditing" />
        </div>
        <div class="actions">
          <button id="done" @click="markAsDone(index)" :disabled="todo.done">
            {{ todo.done ? 'Done' : 'Mark as Done' }}
          </button>
          <button id="delete" @click="deleteTodo(index)">Delete</button>
        </div>
      </li>
    </transition-group>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watch } from 'vue'
import TodoInput from './components/TodoInput.vue'

// Reactive todos array
const todos = ref([])

// Reactive reference to track the index of the task being edited
const editingIndex = ref(null)

// Load the todos from localStorage when the component is mounted
onMounted(() => {
  const savedTodos = localStorage.getItem('todos')
  if (savedTodos) {
    todos.value = JSON.parse(savedTodos) // Load the saved todos
  }
})

// Watch the todos array and save it to localStorage whenever it changes
watch(todos, (newTodos) => {
  localStorage.setItem('todos', JSON.stringify(newTodos))
}, { deep: true }) // Deep watching to detect changes within the array

// Add a new todo to the todos array
const addTodo = (todoText) => {
  todos.value.push({ text: todoText, done: false })
}

// Mark a todo as done
const markAsDone = (index) => {
  todos.value[index].done = true
}

// Try to delete the todo
const deleteTodo = (index) => {
  if (todos.value[index].done) {
    todos.value.splice(index, 1) // Remove if marked as done
  } else {
    alert("Please mark the task as done before deleting it!")
  }
}

const editTodo = (index) => {
  editingIndex.value = index
}

// Exit editing mode and reset the editingIndex
const stopEditing = () => {
  editingIndex.value = null
}

// Computed property to calculate completed todos count
const completedTodosCount = computed(() => {
  return todos.value.filter(todo => todo.done).length
})
</script>

<style>
/* Global styles */
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap'); /* Importing Poppins font */

body {
  font-family: 'Poppins', sans-serif; /* Using Poppins font */
  color: #333; 
}
.header{
  font-size:3rem;
  color: #007a7c;
  font-weight: 600;
}

.app {
  max-width: 800px;
  margin: 0 auto;
  padding: 4rem;
  text-align: center;
  background: #ffffff; /* All white background */
  border-radius: 10px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
}

h1 {
  color: #333; /* Dark color for heading */
  font-family: 'Poppins', sans-serif; /* Consistent font */
}

p {
  margin: 1rem 0;
  font-weight: bold;
}

.todo-list {
  list-style: none;
  padding: 0.5rem;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  background: #f9f9f9;
  margin: 10px 0;
  padding: 10px 15px;
  border-radius: 10px;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

/* Style for completed todo */
.todo-done span {
  text-decoration: line-through;
  color: #888;
}

.todo-item:hover {
  transform: scale(1.02);
}

.actions {
  display: flex;
  gap: 10px;
}

input {
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 100%;
}

#done {
  background-color: #007a7c;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

#delete {
  background-color: #e01e08;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

#done:disabled {
  background-color: #ccc; /* Greyed-out button when disabled */
  cursor: not-allowed;
}

button:hover:enabled {
  background-color: #8e5151;
}

/* Animations for adding and removing todos */
.fade-enter-active, .fade-leave-active {
  transition: all 0.5s ease;
}

.fade-enter {
  opacity: 0;
  transform: translateY(20px);
}

.fade-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}
</style>
