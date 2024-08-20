<template>
  <h1>Hello World!</h1>
  <ButtonCounter />
  <p>Each ButtonCounter has its own state</p>
  <ButtonCounter />
  <br />
  <BlogParent />
  <br />
  <SlotAlertBox>Something bad has happened.</SlotAlertBox>
  <br />
  <DynamicComponentsParent />
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Welcome to Your Vue.js App" />
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import ButtonCounter from "./components/ButtonCounter.vue";
import BlogParent from "./components/passing-props/BlogParent.vue";
import SlotAlertBox from "./components/SlotAlertBox.vue";
import DynamicComponentsParent from "./components/dynamic-components/DynamicComponentsParent.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
    ButtonCounter,
    BlogParent,
    SlotAlertBox,
    DynamicComponentsParent
  },
};
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
