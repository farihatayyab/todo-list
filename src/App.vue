<template>
  <div class="app">
    <h1>📝 TODO LIST</h1>

    <!-- Search & Filter -->
    <div class="search-and-filter">
      <div class="search-group">
        <input 
          type="text" 
          placeholder="Search or add note..." 
          v-model="searchQuery"
          @keyup.enter="addFromSearch"
        />
        <svg class="search-icon" viewBox="0 0 24 24">
          <path d="M15.5 14h-.79l-.28-.27A6.471 6.471 0 0 0 16 9.5 
                   6.5 6.5 0 1 0 9.5 16c1.61 0 3.09-.59 
                   4.23-1.57l.27.28v.79l5 4.99L20.49 
                   19l-4.99-5zm-6 0C7.01 14 5 11.99 
                   5 9.5S7.01 5 9.5 5 14 7.01 14 
                   9.5 11.99 14 9.5 14z"/>
        </svg>
      </div>
      
      <select v-model="filterStatus" class="filter-dropdown">
        <option value="all">ALL</option>
        <option value="completed">Completed</option>
        <option value="pending">Pending</option>
      </select>
    </div>

    <!-- Task List -->
    <ul v-if="filteredTasks.length > 0">
      <li v-for="(task, index) in filteredTasks" :key="index" :class="{ done: task.done }">
        <div class="task-info">
          <input 
            type="checkbox" 
            :checked="task.done" 
            @change="toggleTask(task)" 
          />
          <span class="task-text">{{ task.text }}</span>
        </div>
        <button class="remove-btn" @click="removeTask(index)">✕</button>
      </li>
    </ul>
    <p v-else class="no-task">✨ No tasks yet, add one!</p>

    <!-- Add Task -->
    <div class="input-group">
      <input 
        type="text" 
        placeholder="Add a new task..." 
        v-model="newTask" 
        @keyup.enter="addTask" 
      />
      <button class="add-btn" @click="addTask">+</button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const newTask = ref('');
const tasks = ref([]);
const searchQuery = ref('');
const filterStatus = ref('all');

function addTask() {
  if (newTask.value.trim()) {
    tasks.value.push({ text: newTask.value, done: false });
    newTask.value = '';
  }
}

function addFromSearch() {
  if (searchQuery.value.trim()) {
    tasks.value.push({ text: searchQuery.value, done: false });
    searchQuery.value = '';
  }
}

function toggleTask(task) {
  task.done = !task.done;
}

function removeTask(index) {
  tasks.value.splice(index, 1);
}

const filteredTasks = computed(() => {
  let filtered = tasks.value;

  if (searchQuery.value) {
    const query = searchQuery.value.toLowerCase();
    filtered = filtered.filter(task => 
      task.text.toLowerCase().includes(query)
    );
  }

  if (filterStatus.value === 'completed') {
    filtered = filtered.filter(task => task.done);
  } else if (filterStatus.value === 'pending') {
    filtered = filtered.filter(task => !task.done);
  }

  return filtered;
});
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');

.app {
  width: 100%;
  max-width: 450px;
  padding: 30px;
  background: rgba(25, 25, 30, 0.7);
  border-radius: 20px;
  backdrop-filter: blur(20px);
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
  text-align: center;
  color: #e0e0e0;
}

h1 {
  font-size: 1.8rem;
  font-weight: 600;
  margin-bottom: 25px;
  color: #fff;
}

.search-and-filter {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.search-group {
  position: relative;
  flex: 1;
}

.search-group input {
  width: 100%;
  padding: 10px 10px 10px 40px;
  font-size: 1rem;
  border: none;
  outline: none;
  background: #333;
  color: #e0e0e0;
  border-radius: 10px;
}

.search-group input::placeholder {
  color: #777;
}

.search-icon {
  position: absolute;
  left: 10px;
  top: 50%;
  transform: translateY(-50%);
  width: 20px;
  height: 20px;
  fill: #777;
}

.filter-dropdown {
  padding: 10px 30px 10px 15px;
  margin-left: 50px;
  border: none;
  border-radius: 10px;
  background: #f7f3f3;
  color: #0f0e0e;
  font-size: 0.9rem;
  cursor: pointer;
  appearance: none;
background-image: url('data:image/svg+xml;utf8,<svg fill="%23000000" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><circle cx="12" cy="12" r="10"/><path fill="%23ffffff" d="M8.59 10.59L12 14l3.41-3.41L17 12l-5 5-5-5z"/></svg>');

  background-repeat: no-repeat;
  background-repeat: no-repeat;
  background-position: right 10px center;
  background-size: 12px;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: rgba(255, 255, 255, 0.1);
  padding: 15px;
  border-radius: 15px;
  margin-bottom: 12px;
  transition: 0.3s;
  backdrop-filter: blur(10px);
  box-shadow: 0 6px 25px rgba(0, 0, 0, 0.1);
}

li:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
}

.task-info {
  display: flex;
  align-items: center;
  flex: 1;
}

.task-info input[type="checkbox"] {
  width: 20px;
  height: 20px;
  margin-right: 15px;
  cursor: pointer;
}

.task-text {
  font-size: 1.1rem;
  font-weight: 500;
  color: #fff;
  word-break: break-word;
  text-align: left;
}

.done .task-text {
  text-decoration: line-through;
  color: rgba(255, 255, 255, 0.6);
}

.remove-btn {
  background: rgba(255, 99, 132, 0.9);
  color: #fff;
  border: none;
  border-radius: 4px;   /* rectangle with small rounded corners */
  padding: 6px 12px;    /* thoda spacing */
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  transition: 0.3s;
}

.remove-btn:hover {
  background: #ff4d6d;
  transform: scale(1.05);
}

.input-group {
  display: flex;
  margin-top: 25px;
  /* input aur button ke beech spacing */
}

.input-group input {
  flex: 1;
  padding: 10px 15px;
  font-size: 1rem;
  border: 1px solid rgba(255,255,255,0.4);
  border-radius: 6px; /* soft rounded rectangle */
  outline: none;
  background: rgba(255, 255, 255, 0.15);
  color: #fff;
  transition: 0.3s;
}

.input-group input::placeholder {
  color: rgba(255, 255, 255, 0.7);
}

.input-group input:focus {
  background: rgba(255, 255, 255, 0.25);
  border-color: #6a1b9a;
}

/* Add button styled like remove button (rectangular) */
.add-btn {
  background: #6a1b9a;
  color: white;
  font-size: 1rem;
  font-weight: bold;
  border: none;
  border-radius: 6px;  /* same as input box */
  cursor: pointer;
  transition: transform 0.2s, background 0.3s;
  padding: 10px 20px;
}

.add-btn:hover {
  background: #4a148c;
  transform: scale(1.05);
}


.no-task {
  font-size: 1rem;
  font-style: italic;
  color: #aaa;
  margin: 20px 0;
}
</style>
