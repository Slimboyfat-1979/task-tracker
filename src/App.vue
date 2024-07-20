<template>
    <div class="container">
        <Header v-on:toggle-add-task="toggleAddTask" title="Task Tracker" v-bind:showAddTask="showAddTask"/>
        <div v-show="showAddTask">
            <AddTask @add-task="addTask"></AddTask>
        </div>
        <Tasks v-on:toggle-reminder="toggleReminder" 
               v-on:delete-task="deleteTask" 
               v-bind:tasks="tasks">
        </Tasks>
        <router-view></router-view>
        <Footer></Footer>
    </div>
</template>

<script>

import Header from './components/Header.vue';
import Tasks from './components/Tasks.vue';
import AddTask from './components/AddTask.vue'
import Footer from './components/Footer.vue';

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask,
    Footer
  },
  data() 
    {
        return {
            tasks: [],
            showAddTask: false
        }
    },
    methods: {
        toggleAddTask() {
            this.showAddTask = !this.showAddTask
        },
        async addTask(task) {
            const res = await fetch('api/tasks', {
                method: 'POST',
                headers: {
                    'Content-type': 'application/json',
                },
                body: JSON.stringify(task)
            })
            const data = await res.json()
            this.tasks = [...this.tasks, data]
        },
        async deleteTask(id) {
            const res = await fetch(`api/tasks/${id}`, {
                method: 'DELETE',
            })
            res.status === 200 ? (this.tasks = this.tasks.filter((task) => task.id !== id)) : alert('Error Deleting Task')
        },
        async toggleReminder(id) {
            const taskToToggle = await this.fetchTask(id);
            const updTask = {...taskToToggle, reminder: !taskToToggle.reminder};

            const res = await fetch(`api/tasks/${id}`, {
                method: 'PUT',
                header: {
                    "Content-type": 'application/json'
                },
                body: JSON.stringify(updTask)
            });

            const data = await res.json()
            this.tasks = this.tasks.map((task) => {
               if(task.id === id) {
                    return {...task, reminder: data.reminder}
                }else {
                    return task;
                }
            })
        },
        async fetchTasks() {
            const response = await fetch('api/tasks');
            const data = await response.json();
            return data;
        },
        async fetchTask(id){
            const response = await fetch(`api/tasks/${id}`)
            const data = response.json();
            return data;
        }
    },
    async created() {
        this.tasks = await this.fetchTasks();
    }
}
</script>

<style>
    * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    }

    body {
    font-family: 'Poppins', sans-serif;
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
