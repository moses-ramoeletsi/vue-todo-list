<template>
  <div id="app">
    <h1 class="title">Todo List</h1>
    <form @submit.prevent="addTask" class="task-form">
      <input v-model="newTask" class="task-input" placeholder="Enter a new task" />
      <button class="add-button">Add Task</button>
    </form>
    <ul class="task-list">
      <li v-for="(task, index) in task" :key="index" class="task-item">
        {{ task }}
        <button @click="editTask(index)" class="edit-button">Edit</button>
        <button @click="deleteTask(index)" class="delete-button">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default  {
  data() {
    return {
      newTask: '',
      task: [],
      selectedTaskIndex: -1
    };
  },

  created(){
    const storedTasks = localStorage.getItem('task');
    if(storedTasks){
      try{
        this.task = JSON.parse(storedTasks);

      }catch (error){
        console.log('Error parsing task', error);
        localStorage.removeItem('task')
      }
    }
  },
  
  methods:{
    addTask(){
      if(this.newTask.trim() !== ''){
        this.task.push(this.newTask);
        this.newTask = '';
        this.saveTask();

      }
    },
    editTask(index){
      this.selectedTaskIndex = index;
      const editedTask = prompt('Edit task:', this.task[index]);
      if(editedTask !== null){
        this.task[index] = editedTask;
        this.selectedTaskIndex = -1;
        this.saveTask();
        console.log(editedTask)
      }
    },
    deleteTask(index){
      if(window.confirm ('Are you sure you wnat to delete this task')){
        this.task.splice(index, 1);
        this.saveTask();
      }
    },

    saveTask(){
      localStorage.setItem('task', JSON.stringify(this.task));
    }
  }
}

</script>

<style>
.title {
  text-align: center;
  margin-bottom: 20px;
}

.task-form {
  text-align: center;
  margin-bottom: 20px;
}

.task-input {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-right: 10px;
}

.add-button {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
}

.add-button:hover {
  background-color: #45a049;
}

.task-list {
  list-style: none;
  padding: 0;
}

.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  border: 1px solid #ccc;
  padding: 8px;
  margin-bottom: 10px;
  border-radius: 4px;
}

.edit-button,
.delete-button {
  background-color: #f44336;
  color: white;
  border: none;
  padding: 4px 8px;
  border-radius: 4px;
  cursor: pointer;
  margin-left: 10px;
}

.edit-button:hover,
.delete-button:hover {
  background-color: #d32f2f;
}
</style>
