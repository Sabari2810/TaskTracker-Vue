<template>
  <header>
    <h1>{{title}}</h1>
    <Button
      @btn-click="$emit('toggle-add-task')"
      :text="showAddTask ? 'Close' : 'Add Task' "
      :color="showAddTask ? 'red' : 'Green' "
    />
  </header>
</template>

 
<script>
import Button from "./Button";

export default {
  name: "Header",
  props: {
    title: String,
    showAddTask: Boolean
  },
  components: {
    Button
  }
};
</script>

<style scoped>
header {
  display: flex;
  align-items: center;
  flex-direction: row;
  justify-content: space-between;
  margin-bottom: 20px;
}
</style>
