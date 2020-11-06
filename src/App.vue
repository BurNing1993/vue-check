<template>
  <div id="app-container">
    <Check />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Check from "./Check.vue";

export default defineComponent({
  name: "App",
  components: {
    Check,
  },
});
</script>
