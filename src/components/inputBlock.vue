<template>
  <div id="input">

    <div id="wrap">
      <pre id="pre">
        <span>{{inputMessage}}</span><br></pre>

      <textarea id="msgInput"
        placeholder="'你好，华华！'"
        maxlength="1000"
        v-model="inputMessage"
        v-on:keydown.enter.prevent="sendAndAppendMsg">
      </textarea>
    </div>

    <span id="iconBlock"
      v-on:click="sendAndAppendMsg">
      <img v-if="iconObj.clickable"
        src="../assets/plane_blue.png">
      <img v-if="iconObj.unclickable"
        src="../assets/plane_gray.svg">
    </span>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      inputMessage: ""
    };
  },
  computed: {
    iconObj: function() {
      return {
        unclickable: this.inputMessage.trim() === "",
        clickable: this.inputMessage.trim() !== ""
      };
    }
  },
  methods: {
    sendAndAppendMsg: function() {
      if (this.inputMessage.trim() !== "") {
        this.$emit("appendMsgToMsgBoard", {
          whichSide: "right",
          msgContent: this.inputMessage
        });

        this.inputMessage = "";
      }
    }
  }
};
</script>

<style scoped>
#input {
  background-color: #ffffff;
  box-shadow: 0 -2px 5px rgba(0, 0, 0, 0.16);

  position: relative;
  padding: 8px 40px 8px 8px;
}

#wrap {
  position: relative;
}

#msgInput,
#pre {
  box-sizing: border-box;
  min-height: 24px;
  width: 100%;
  font-size: 16px;
}

#pre {
  margin: 0;
  display: block;
  visibility: hidden;
}

#pre > span {
  white-space: pre-wrap;
}

#msgInput {
  border: 0px;
  height: 100%;

  position: absolute;
  top: 0;
  left: 0;

  outline: none;
  resize: none;
  overflow: hidden;

  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#msgInput::-webkit-input-placeholder {
  color: #c2c5cc;
}

#iconBlock {
  display: inline-block;
  width: 40px;
  height: 40px;

  position: absolute;
  bottom: 0;
  right: 0;

  display: flex;
  align-items: center;
  justify-content: center;
}

#iconBlock > img {
  width: 24px;
}
</style>

