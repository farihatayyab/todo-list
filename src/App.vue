<template>
<div id="app">
  <h1>📝 Todo List</h1>
<input type="text" placeholder="add todo" v-model="newTask" @keyup.enter="addTask"/>
<button @click="addtask">add</button>

<ul>
   <li v-for="(task, index) in tasks" :key="index">
   <span :class="{ done: task.done }" @click="toggleTask(task)">
          {{ task.text }}
        </span>
        <button @click="removetask(index)">remove</button>
   </li>




</ul>
  </div>
  </template>


<script>

export default {
  name: 'App',
  data(){
  return{
    newTask: '',
    tasks: []
  }
  },
  methods:{
    addtask(){
      if(this.newTask.trim()){
        this.tasks.push({text :this.newTask,done:false})
      }
      
    },
      toggleTask(task) {
      task.done = !task.done;
    },
    removetask(index){
      this.tasks.splice(index,1)
    }
  }
};
 
 

</script>

<style>
.app {
  max-width: 400px;
  margin: 50px auto;
  text-align: center;
  font-family: Arial, sans-serif;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  margin: 5px 0;
  padding: 8px;
  background: #f4f4f4;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-radius: 6px;
}
.done {
  text-decoration: line-through;
  color: gray;
}
</style>


