<template>
  <div class="container">
    <div class="masthead">
      <h1 class="masthead-title">
        <a href="/">WeLikeTheSpeech</a>
      </h1>
      <div class="masthead-nav">
        <a href="/#leaderboard">leaderboard</a>
      </div>
    </div>
    <div class="page-content">
      <Home />
    </div>
  </div>
</template>

<script>
import Home from "./components/Home.vue";

export default {
  name: "App",
  components: {
    Home,
  },
};
</script>

<style>
@import "./assets/style.css";

#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
