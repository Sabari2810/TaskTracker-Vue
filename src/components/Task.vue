<template>
  <div @dblclick="$emit('change-reminder',task.id)" :class="[task.reminder ? 'reminder' : '','task']">
    <h3>
      {{task.text}}
      <i @click="$emit('delete-task',task.id)" class="fas fa-times"></i>
    </h3>
    <p>{{task.day}}</p>
  </div>
</template>

<script>
export default {
  name: "Task",
  props: {
    task: Object
  },
};
</script>


<style scoped>
.fas {
  cursor: pointer;
  color: red;
}
.task {
  background-color: #f4f4f4;
  margin: 5px;
  padding: 10px;
}

.task.reminder {
  border-left: 5px solid green;
}
h3 {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>>