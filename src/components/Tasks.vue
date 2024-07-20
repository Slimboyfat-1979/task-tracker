<template>
    <div v-for="task in tasks" v-bind:key="task.id">
        <Task v-on:toggle-reminder="$emit('toggle-reminder', task.id)" v-on:delete-task="$emit('delete-task', task.id)" v-bind:task="task"></Task>
    </div>
</template>

<script>
import Task from './Task.vue'
    export default {
        name: 'TheTasks',
        props: {
            tasks: Array
        },
        components: {
            Task
        },
        emits: ['delete-task', 'toggle-reminder']
    }
</script>