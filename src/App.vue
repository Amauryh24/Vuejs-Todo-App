<template>
  <div id="app" class="container">
    <img src="./assets/Clock-01.png" class="logo" />
    <todo-list></todo-list>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld";
import TodoList from "./components/TodoList";
export default {
  name: "App",
  components: {
    HelloWorld,
    TodoList
  }
};
</script>

<style>
body {
  background: linear-gradient(
    157deg,
    rgba(77, 184, 128, 1) 0%,
    rgba(69, 145, 116, 1) 52%,
    rgba(53, 73, 94, 1) 100%
  );
  background-repeat: no-repeat;
  background-size: cover;
}

.logo {
  display: block;
  margin: 20px auto;
  height: 100px;
}

#app {
  width: 80%;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  font-size: 24px;
  padding: 0;
  margin: 0 auto;
}
</style>
