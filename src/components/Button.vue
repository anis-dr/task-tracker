<template>
  <button class="btn" :style="{ background: color }" @click="onClick()">
    {{ text }}
  </button>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Button",
  props: {
    text: String,
    color: String,
  },
  methods: {
    onClick(): void {
      this.$emit("btn-click");
    },
  },
});
</script>

<style></style>
