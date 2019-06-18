<template>
  <div id="app">
    <msgBubble v-bind:which-side="whichSide"
      v-bind:content="msgContent"></msgBubble>
  </div>
</template>

<script>
import msgBubble from "./components/msgBubble.vue";

export default {
  name: "app",
  data: function() {
    return {
      whichSide: "right",
      msgContent:
        "照道理说，他也不能回到故国去，或许已经死在半路上了吧？可能再也不会听到关于那家伙的事情了。"
    };
  },
  components: {
    msgBubble
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
