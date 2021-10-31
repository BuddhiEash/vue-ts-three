<template>
  <threejsmanmodel></threejsmanmodel>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Threejsmanmodel from "./components/Threejsmanmodel.vue";

export default defineComponent({
  name: "App",
  components: {
    Threejsmanmodel,
  },
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
