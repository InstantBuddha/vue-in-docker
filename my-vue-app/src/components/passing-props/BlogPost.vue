<script setup>
defineProps({
  title: String,
  content: String,
  author: String,
});
</script>

<template>
  <div class="blog-post">
    <h2>{{ title }}</h2>
    <p>{{ content }}</p>
    <small>Written by: {{ author }}</small>
    <button @click="$emit('enlarge-text')">Enlarge text</button>
  </div>
</template>

<style scoped>
.blog-post {
  border: 1px solid #ddd;
  padding: 16px;
  margin-bottom: 16px;
}
</style>