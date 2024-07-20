<template>
  <header>
    <h1>{{ title }}</h1>
    <Button
      v-on:btn-click="$emit('toggle-add-task')"
      v-bind:text="showAddTask ? 'Close' : 'Add Task'"
      v-bind:color="showAddTask ? 'red' : 'green'"
    ></Button>
  </header>
</template>

<script>
import Button from "./Button.vue";

export default {
  name: "TheHeader",
  props: {
    title: String,
    showAddTask: Boolean,
  },
  components: {
    Button,
  },
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
