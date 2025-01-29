<template>
  <div class="container">
    <h1 class="title">Task Manager</h1>

    <!-- Form -->
    <div class="form-section">
      <h2 class="subtitle">Add New Task</h2>
      <form @submit.prevent="addTask" class="form">
        <div class="form-group">
          <label for="title" class="label">Title</label>
          <input type="text" id="title" v-model="newTask.title" class="input" required />
        </div>
        <div class="form-group">
          <label for="description" class="label">Description</label>
          <textarea id="description" v-model="newTask.description" class="textarea" required></textarea>
        </div>
        <div class="form-group">
          <input type="checkbox" id="isDone" v-model="newTask.isDone" class="checkbox" />
          <label for="isDone" class="label-inline">Mark as Done</label>
        </div>
        <button type="submit" class="button">Add Task</button>
      </form>
    </div>

    <!-- Task List -->
    <div class="task-list">
      <h2 class="subtitle">Task List</h2>
      <div v-if="tasks.length" class="cards">
        <div 
          v-for="task in tasks" 
          :key="task.id" 
          class="card">
          <div>
            <h3 class="card-title" :class="{ 'line-through': task.isDone }">{{ task.title }}</h3>
            <p class="card-description">{{ task.description }}</p>
          </div>
          <div class="card-actions">
            <button @click="editTask(task)" class="button-edit">Edit</button>
            <button @click="deleteTask(task.id)" class="button-delete">Delete</button>
          </div>
        </div>
      </div>
      <p v-else class="no-tasks">No tasks available. Add some!</p>
    </div>
  </div>
</template>

<script>
import socket from "@/services/socket";

export default {
  data() {
    return {
      tasks: [],
      newTask: {
        id: null,
        title: '',
        description: '',
        isDone: false,
      },
      editMode: false,
    };
  },
  mounted(){
    this.getAllTasks();

    socket.on("findAllTodoInFE", (payload) => {
        this.tasks = payload;
      });
  },
  methods: {
    getAllTasks() {
      socket.emit("findAllTodoInBE");
    },
    addTask() {
      event.preventDefault();

      if (this.editMode) {
        // const index = this.tasks.findIndex(task => task.id === this.newTask.id);
        // if (index !== -1) {
        //   // Directly update the task in the array
        //   this.tasks[index] = { ...this.newTask };
        // }
        
        socket.emit("updateTodoInBE", this.newTask);
        this.editMode = false; // Exit edit mode

      } else {
         const newTask = { ...this.newTask, id: Date.now() };
         socket.emit("createTodoInBE", newTask);
      }
      this.resetForm();
    },
    editTask(task) {
      this.newTask = { ...task };
      this.editMode = true;
    },
    deleteTask(id) {
      socket.emit("removeTodoInBE", id);

      // this.tasks = this.tasks.filter(task => task.id !== id);
    },
    resetForm() {
      this.newTask = {
        id: null,
        title: '',
        description: '',
        isDone: false,
      };
    },
  },
};
</script>

<style>
.container {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  font-family: Arial, sans-serif;
}
.title {
  font-size: 24px;
  font-weight: bold;
  text-align: center;
  margin-bottom: 20px;
}
.subtitle {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 10px;
}
.form {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 20px;
}
.form-group {
  display: flex;
  flex-direction: column;
}
.label {
  font-size: 14px;
  margin-bottom: 5px;
}
.label-inline {
  font-size: 14px;
  margin-left: 5px;
}
.input, .textarea {
  padding: 8px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
.textarea {
  resize: none;
  height: 60px;
}
.checkbox {
  margin-right: 5px;
}
.button {
  padding: 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.button:hover {
  background-color: #0056b3;
}
.task-list {
  margin-top: 20px;
}
.cards {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.card {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.card-title {
  font-size: 16px;
  font-weight: bold;
}
.card-description {
  font-size: 14px;
  color: #555;
}
.card-actions {
  display: flex;
  gap: 5px;
}
.button-edit {
  padding: 5px 10px;
  background-color: #ffc107;
  color: black;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.button-edit:hover {
  background-color: #e0a800;
}
.button-delete {
  padding: 5px 10px;
  background-color: #dc3545;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.button-delete:hover {
  background-color: #c82333;
}
.no-tasks {
  font-size: 14px;
  color: #777;
  text-align: center;
}
.line-through {
  text-decoration: line-through;
}
</style>
