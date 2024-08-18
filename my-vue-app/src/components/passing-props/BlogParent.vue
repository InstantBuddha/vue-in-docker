<script setup>
import { ref } from "vue";
import BlogPost from "./BlogPost.vue";

const posts = ref([
  {
    id: 1,
    title: "My Journey with Vue",
    content: "Vue.js is an amazing framework...",
    author: "Alice",
  },
  {
    id: 2,
    title: "Why I Love JavaScript",
    content: "JavaScript allows me to build...",
    author: "Bob",
  },
  {
    id: 3,
    title: "The Future of Web Development",
    content: "Web development is evolving...",
    author: "Charlie",
  },
]);

const postFontSize = ref(1);
</script>

<template>
  <div :style="{ fontSize: postFontSize + 'em' }">
    <h2>Created with for in</h2>
    <BlogPost
      v-for="post in posts"
      :key="post.id"
      :title="post.title"
      :content="post.content"
      :author="post.author"
      @enlarge-text="postFontSize += 0.1"
    />
    <h2>Passing props individually</h2>
    <p>I did not pass the @enlarge-text event listener, so the button obviously does not work.</p>
    <BlogPost 
      title="Unique title"
      content="Lorem Ipsum stuff"
      author="John Doe"
    />
  </div>
</template>
