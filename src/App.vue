<template>
  <div id="app" class="app">
    <h1>📝 Todo List</h1>

    <div class="input-group">
      <input 
        type="text" 
        placeholder="Add a new task..." 
        v-model="newTask" 
        @keyup.enter="addTask" 
      />
      <button @click="addTask">+</button>
    </div>

    <ul>
      <li v-for="(task, index) in tasks" :key="index" :class="{ done: task.done }">
        <div class="task-text" @click="toggleTask(task)">
          {{ task.text }}
        </div>
        <button class="remove-btn" @click="removeTask(index)">✕</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const newTask = ref('');
const tasks = ref([]);

function addTask() {
  if (newTask.value.trim()) {
    tasks.value.push({ text: newTask.value, done: false });
    newTask.value = '';
  }
}

function toggleTask(task) {
  task.done = !task.done;
}

function removeTask(index) {
  tasks.value.splice(index, 1);
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');

body {
  background: linear-gradient(135deg, #010e0f 0%, #03295f4f 100%);
  font-family: 'Poppins', sans-serif;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0;
}

/* App container */
.app {
  width: 350px;
  padding: 30px 25px;
  background: rgba(255, 255, 255, 0.15);
  border-radius: 25px;
  backdrop-filter: blur(15px);
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.12);
  text-align: center;
  color: #fff;
}

/* Heading */
h1 {
  font-size: 2.2rem;
  font-weight: 600;
  margin-bottom: 30px;
  text-shadow: 0 2px 5px rgba(0,0,0,0.2);
}

/* Input Group */
.input-group {
  display: flex;
  margin-bottom: 30px;
  border-radius: 15px;
  overflow: hidden;
  box-shadow: 0 4px 20px rgba(0,0,0,0.2);
}

input {
  flex: 1;
  padding: 12px 20px;
  font-size: 1rem;
  border: none;
  outline: none;
  background: rgba(255,255,255,0.25);
  color: #fff;
  transition: 0.3s;
}

input::placeholder {
  color: rgba(255,255,255,0.7);
}

input:focus {
  background: rgba(255,255,255,0.35);
}

button {
  background: #ff6b81;
  font-size: 1.3rem;
  border: none;
  cursor: pointer;
  transition: transform 0.2s, background 0.3s;
}

button:hover {
  transform: scale(1.1);
  background: #ff4757;
  box-shadow: 0 5px 15px rgba(0,0,0,0.3);
}

/* Task List */
ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: rgba(255,255,255,0.15);
  padding: 12px 20px;
  border-radius: 20px;
  margin-bottom: 15px;
  transition: 0.3s;
  backdrop-filter: blur(10px);
  box-shadow: 0 6px 25px rgba(0,0,0,0.1);
}

li:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 30px rgba(0,0,0,0.15);
}

/* Task text */
.task-text {
  cursor: pointer;
  font-size: 1.5rem;
  font-weight: 500;
  color: #fff;
  word-break: break-word;
}

/* Done state */
.done .task-text {
  text-decoration: line-through;
  color: rgba(255,255,255,0.6);
}

/* Remove button */
.remove-btn {
  background: rgba(255, 99, 132, 0.6);
  border-radius: 50%;
  padding: 5px 10px;
  font-size: 1rem;
  color: #fff;
  border: none;
  transition: 0.3s;
}

.remove-btn:hover {
  background: #ff4d6d;
  transform: scale(1.2);
}
</style>
