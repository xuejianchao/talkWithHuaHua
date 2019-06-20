<template>
  <div id="app">
    <header id="header">
      华华
    </header>
    <div id="msgBoard">
      <msgBlock class="msgBlock"
        v-for="msg in messages"
        v-bind:msg="msg"
        v-bind:key="msg.id">
      </msgBlock>
    </div>
    <div id="inputBlock">
      <inputBlock v-on:appendMsgToMsgBoard="appendMsgToMsgBoard"></inputBlock>
    </div>
  </div>
</template>

<script>
import msgBlock from "./components/msgBlock.vue";
import inputBlock from "./components/inputBlock.vue";

export default {
  name: "app",
  data: function() {
    return {
      messages: [
        {
          id: "1",
          whichSide: "right",
          msgContent:
            "照道理说，他也不能回到故国去，或许已经死在半路上了吧？可能再也不会听到关于那家伙的事情了。"
        },
        {
          id: "2",
          whichSide: "left",
          msgContent: "你说的有道理，可是这和我有什么关系呢？"
        },
        {
          id: "3",
          whichSide: "right",
          msgContent: "haoba"
        },
        {
          id: "4",
          whichSide: "left",
          msgContent: "你说的有道理，可是这和我有什么关系呢？"
        },
        {
          id: "5",
          whichSide: "right",
          msgContent: "haoba"
        }
      ]
    };
  },
  components: {
    msgBlock,
    inputBlock
  },
  methods: {
    appendMsgToMsgBoard: function(halfDoneMsgObj) {
      let lastId = this.messages[this.messages.length - 1].id;
      halfDoneMsgObj.id = lastId + 1;

      this.messages.push(halfDoneMsgObj);

      // 向后端发送消息
    }
  }
};
</script>

<style>
html,
body {
  margin: 0px;
  padding: 0px;
  background-color: bisque;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  color: #2c3e50;
  background-color: #eceff1;

  width: 60%;
  height: 100vh;

  box-sizing: border-box;
  position: relative;
  padding-top: 40px;
  margin: auto;

  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

#header {
  line-height: 40px;
  height: 40px;
  text-align: center;

  background-color: #ffffff;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.16);

  position: absolute;
  top: 0;
  width: 100%;
}

#msgBoard {
  padding: 16px;

  display: flex;
  flex-direction: column;

  flex-shrink: 1;
  overflow: auto;
}

#inputBlock {
  flex-shrink: 0;
  max-height: 90vh;
  overflow: auto;
}
</style>
