<template>
  <header>
    <h1>{{ title }}</h1>
    <Button
      v-show="homePage"
      :text="showAddTask ? 'Close' : 'Add Task'"
      :color="showAddTask ? 'red' : 'green'"
      @btn-click="$emit('toggle-add-task')"
    ></Button>
  </header>
</template>

<script lang="ts">
import Button from "./Button.vue";
import { defineComponent } from "vue";

export default defineComponent({
  name: "Header",
  components: {
    Button,
  },
  props: {
    title: String,
    showAddTask: Boolean,
  },
  computed: {
    homePage(): boolean {
      return this.$route.path === "/";
    },
  },
});
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
