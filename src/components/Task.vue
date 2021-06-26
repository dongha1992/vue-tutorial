<template>
  <div
    @click="$emit('toggle-reminder', task.id)"
    :class="[task.reminder ? 'reminder' : '', 'task']"
  >
    <h3>{{ task.text }}</h3>
    <Button @click="$emit('delete-task', task.id)" text="Delete" color="red" />
  </div>
</template>

<script>
import Button from './Button.vue';

export default {
  name: 'Task',
  props: {
    task: Object,
  },
  components: { Button },
};
</script>

<style scoped>
.task {
  display: flex;
  justify-content: space-between;
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.task.reminder {
  border-left: 5px solid green;
}
</style>
