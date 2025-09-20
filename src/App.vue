<template>
  <div id="app" class="app">
    <h1>📝 Todo List</h1>
    
   
    <div class="input-group">
      <input 
        type="text" 
        placeholder="Add a new task" 
        v-model="newTask" 
        @keyup.enter="addTask" 
      />
      <button @click="addTask">Add</button>
    </div>


    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        <span 
          :class="{ done: task.done }" 
          @click="toggleTask(task)"
        >
          {{ task.text }}
        </span>
        <button @click="removeTask(index)">Remove</button>
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

.app {
  max-width: 450px;
  margin: 50px auto;
  text-align: center;
  font-family: Arial, sans-serif;
}


.input-group {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

input {
  padding: 10px;
  width: 70%;
  border-radius: 5px 0 0 5px;
  border: 1px solid #ccc;
  font-size: 20px;
}

button {
  padding: 10px 15px;
  border: none;
  border-radius: 0 5px 5px 0;
  background-color: #beceeb;
  color: white;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #2563eb;
}

ul {
  list-style: none;
  padding: 0;
  margin-top: 20px;
}

li {
  margin: 8px 0;
  padding: 10px;
  background: #eeb6b6;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-radius: 6px;
  transition: background 0.3s;
}

li:hover {
  background: #e0e0e0;
}

li span {
  cursor: pointer;
  font-size: 16px;
}

/* Done state with strike-through */
.done {
  text-decoration: line-through;
  color: rgb(180, 46, 46);
}
</style>
