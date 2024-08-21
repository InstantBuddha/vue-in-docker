<script setup>
import BasicsContainer from "./components/BasicsContainer.vue";
import ComponentsExamples from "./components/ComponentsExamples.vue";
</script>

<template>
  <BasicsContainer />
  <hr class="dashed-separator" />
  <ComponentsExamples />
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.dashed-separator {
  margin: 5rem 1rem;
  border-top: 3px dashed red;
}
</style>
