<template>
  <div v-show="showAddTask">
    <AddTask @add-task="addTask" :tasksLength="tasks.length" />
  </div>
  <Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>

<script>
import Tasks from '../components/Tasks.vue';
import AddTask from '../components/AddTask.vue';
import { BASE_URL } from '../config/config';

export default {
  name: 'Home',
  components: {
    Tasks,
    AddTask,
  },
  props: {
    showAddTask: Boolean,
  },

  data() {
    return { tasks: [] };
  },
  methods: {
    async addTask(task) {
      const res = await fetch(`${BASE_URL}/tasks`, {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task),
      });
      const data = await res.json();
      this.tasks = [...this.tasks, data];
    },
    async deleteTask(id) {
      if (confirm('sure?')) {
        const res = await fetch(`${BASE_URL}/tasks/${id}`, {
          method: 'DELETE',
        });

        res.status === 200
          ? (this.tasks = this.tasks.filter(task => task.id !== id))
          : alert('failed to delete');
      }
    },

    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const updatedTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

      const res = await fetch(`${BASE_URL}/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updatedTask),
      });

      const data = await res.json();

      this.tasks = this.tasks.map(task =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      );
    },

    async fetchTasks() {
      const res = await fetch(`${BASE_URL}/tasks`);
      if (res) {
        const data = await res.json();
        return data;
      }
    },

    async fetchTask(id) {
      const res = await fetch(`${BASE_URL}/tasks/${id}`);
      if (res) {
        const data = await res.json();
        return data;
      }
    },
  },

  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>

<style scoped></style>
