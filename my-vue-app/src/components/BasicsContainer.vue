<script setup>
import HelloWorld from "./basics/HelloWorld.vue";
import ButtonCounter from "./basics/ButtonCounter.vue";
import BlogParent from "./basics/passing-props/BlogParent.vue";
import SlotAlertBox from "./basics/SlotAlertBox.vue";
import DynamicComponentsParent from "./basics/dynamic-components/DynamicComponentsParent.vue";
</script>

<template>
  <h1>The Basics</h1>
  <ButtonCounter />
  <p>Each ButtonCounter has its own state</p>
  <ButtonCounter />
  <br />
  <BlogParent />
  <br />
  <SlotAlertBox>Something bad has happened.</SlotAlertBox>
  <br />
  <DynamicComponentsParent />
  <img alt="Vue logo" src="../assets/logo.png" />
  <HelloWorld msg="Welcome to Your Vue.js App" />
</template>
