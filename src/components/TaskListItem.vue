<template>
  <div class="task">
    <TaskListItemStatus :task="task" />
    <input type="text" v-model="task.text" v-autoresize-width="200" />
    <span class="time-duration">[{{ task.date | taskTime }}{{ task.duration | taskDuration }}]</span>
    <button @click="deleteTask" class="delete-task">
      <IconTrush></IconTrush>
    </button>
  </div>
</template>

<script>
import { tasksStore } from "../store/tasksStore";
import { AutoReszeWidth } from "../directives/AutoResizeWidth";
import IconTrush from "./IconTrush";
import TaskListItemStatus from "./TaskListItemStatus";

export default {
  props: ["task"],
  components: {
    IconTrush,
    TaskListItemStatus
  },
  directives: {
    AutoReszeWidth
  },
  methods: {
    deleteTask() {
      tasksStore.deleteTask(this.task);
    }
  },
  filters: {
    taskTime: function(value) {
      const dateNumber = new Date(value);

      let hours = dateNumber.getHours().toString();
      let minutes = dateNumber.getMinutes().toString();

      if (hours.length == 1) hours = "0" + hours;
      if (minutes.length == 1) minutes = "0" + minutes;

      return hours + ":" + minutes;
    },
    taskDuration: function(value) {
      return value ? " | " + value : "";
    }
  }
};
</script>

<style scoped>
.task {
  margin: 1rem 0 0 1rem;
  position: relative;
}

input[type="text"] {
  background-color: transparent;
  padding: 0.5rem;
  border: none;
  color: var(--clr-accent);
  margin-left: 2.1rem;
  font-size: var(--font-size-normal);
}

.delete-task {
  border: none;
  padding: 0.4rem;
  margin-left: 10px;
}

.delete-task svg {
  vertical-align: middle;
  fill: var(--clr-accent);
  transition: 0.3s transform ease-in-out;
}

.delete-task:hover svg {
  transform: scale(1.25, 1.25);
}

.time-duration {
  padding-left: 0.8rem;
  font-size: 0.8rem;
}
</style>