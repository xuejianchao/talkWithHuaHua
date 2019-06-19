<template>
  <div id="app">
    <div id="msgBoard">
      <msgBlock class="msgBlock"
        v-for="msg in messages"
        v-bind:msg="msg"
        v-bind:key="msg.id">
      </msgBlock>
    </div>

    <inputBlock v-on:appendMsgToMsgBoard="appendMsgToMsgBoard"></inputBlock>
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

  width: 60%;
  margin: auto;
}

#msgBoard {
  padding: 16px;
  background-color: #eceff1;

  display: flex;
  flex-direction: column;
}
</style>
