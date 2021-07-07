<template>
  <AddTask v-show="showAddTask" @add-task="addTask" />
  <Tasks
    :tasks="tasks"
    @delete-task="deleteTask"
    @toggle-reminder="toggleReminder"
  ></Tasks>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Tasks from "../components/Tasks.vue";
import AddTask from "../components/AddTask.vue";

export type Task = {
  id: string | undefined;
  text: string;
  day: string;
  reminder: boolean;
};

export default defineComponent({
  name: "Home",
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    AddTask,
  },
  data(): { tasks: Task[] } {
    return { tasks: [] };
  },
  methods: {
    async deleteTask(id: string) {
      if (confirm("Are you sure you want to delete this task?")) {
        const res = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
        });

        res.status == 200
          ? (this.tasks = this.tasks.filter((t) => t.id !== id))
          : alert("Error deleting this task.");
      }
    },
    async toggleReminder(id: string) {
      const taskToToggle = await this.fetchTask(id);
      const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder };
      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updTask),
      });
      const data = await res.json();
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      );
    },
    async addTask(task: Task) {
      const res = await fetch("api/tasks", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(task),
      });

      const data = await res.json();

      this.tasks = [...this.tasks, data];
    },
    async fetchTasks(): Promise<Task> {
      const res = await fetch("api/tasks");
      return await res.json();
    },
    async fetchTask(id: string): Promise<Task> {
      const res = await fetch(`api/tasks/${id}`);
      return await res.json();
    },
  },
  async created() {
    // eslint-disable-next-line @typescript-eslint/ban-ts-comment
    /* @ts-ignore */
    this.tasks = await this.fetchTasks();
  },
});
</script>
