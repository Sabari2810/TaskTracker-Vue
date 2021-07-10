<template>
  <div class="container">
    <Header @toggle-add-task="toggleAddTask" title="Task Tracker" :showAddTask="showAddTask" />
    <div v-if="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <Tasks @change-reminder="toggleReminder" @delete-task="onDelete" :tasks="tasks" />
    <Footer/>
  </div>
</template>

<script>
import Header from "./components/Header";
import Tasks from "./components/Tasks";
import AddTask from "./components/AddTask";
import Footer from "./components/Footer";

export default {
  name: "App",
  components: {
    Header,
    Footer,
    Tasks,
    AddTask
  },
  methods: {
    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },
    async onDelete(id) {
      await this.deleteTaskFromServer(id);
      this.tasks = this.tasks.filter(task => task.id !== id);
    },
    async toggleReminder(id) {
      const taskToUpdate = await this.fetchTask(id);
      const update = {...taskToUpdate,reminder : !taskToUpdate.reminder}
      const res = await fetch(`http://localhost:5000/tasks/${id}`,{
        method : "PUT",
        headers : {
          "content-type" : "application/json"
        },
        body : JSON.stringify(update)
      });
      const data = await res.json()
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      );
    },
    async addTask(task) {
      const res = await this.addTaskServer(task);
      const data = await res.json()
      this.tasks = [...this.tasks, data];
    },
    async getTasks() {
      const res = await fetch("http://localhost:5000/tasks");
      const tasks = await res.json();
      return tasks;
    },
    async fetchTask(id){
      const res = await fetch(`http://localhost:5000/tasks/${id}`);
      const task = await res.json();
      return task;
    },
    async addTaskServer(task){
      return await fetch("http://localhost:5000/tasks/",{
        method : "POST",
        body : JSON.stringify(task),
        headers : {
          "Content-Type":"application/json"
        }
      })
    },
    async deleteTaskFromServer(id){
      const res = await fetch(`http://localhost:5000/tasks/${id}`,{
        method : "DELETE"
      })
    }
  },
  data() {
    return {
      tasks: [],
      showAddTask: false
    };
  },
  async created() {
    // getTasks();
    this.tasks = await this.getTasks();
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: "Poppins", sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
