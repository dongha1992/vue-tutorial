<template>
  <form @submit="onSubmit" class="add-form">
    <div class="form-control">
      <label>Task</label>
      <input type="text" v-model="text" name="text" placeholder="Add Task" />
    </div>
    <div class="form-control">
      <label>Day and Time</label>
      <input
        type="text"
        v-model="day"
        name="day"
        placeholder="Add Day and Time"
      />
    </div>
    <div class="form-control form-control-check">
      <label>Set Reminder</label>
      <input type="checkbox" v-model="reminder" name="reminder" />
    </div>
    <input type="submit" value="Save Task" class="btn btn-block" />
  </form>
</template>

<script>
export default {
  name: 'AddTask',
  data() {
    return {
      text: 'text',
      day: '',
      reminder: false,
    };
  },
  props: {
    tasksLength: Number,
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();
      if (!this.text) {
        alert('should be filled');
        return;
      }
      const newTask = {
        id: this.tasksLength + 1,
        text: this.text,
        day: this.day,
        reminder: this.reminder,
      };
      console.log(newTask, 'new');
    },
  },
};
</script>

<style scoped>
.add-from {
  margin-bottom: 40px;
}

.form-control {
  margin: 20px 0;
}

.form-control label {
  display: block;
}

.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}
.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.form-control-check label {
  flex: 1;
}

.form-control-check input {
  flex: 2;
  height: 20px;
}
</style>
