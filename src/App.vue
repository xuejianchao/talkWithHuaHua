<template>
  <div id="wrapperForTwoPages"
    v-bind:class="wrapperForTwoPagesClassObj">
    <section id="chatPage"
      class="page">
      <section id="chatWindow">
        <header id="header">
          华华
          <i id="toAnalyzePage"
            v-on:click="toRightPage"></i>
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
      </section>
    </section>

    <section id="analyzePage"
      class="page">

      <section id="msgList">
        <h2>
          <i id="toChatPage"
            v-on:click="toLeftPage"></i>
          发送的消息
        </h2>
        <div v-for="json in returnJSONs"
          v-bind:key="json.id"
          v-on:click="setThisMsgActive(json.id)">
          <msgBubble v-bind:content="json.message"
            which-side="right"></msgBubble>
        </div>
      </section>

      <section id="analyzeResults">
        <div id="replyBlock">
          <h2>华华回复</h2>
          <msgBubble v-bind:content="replyFromHuaHua"
            which-side="left"></msgBubble>
        </div>
        <div id="graphBlock">
          <div id="typeBlock">
            <h2>分类分析结果</h2>
            <multBarGraph v-if="returnJSONs[activeJSONIndex]"
              v-bind:types="returnJSONs[activeJSONIndex].types"></multBarGraph>
            <p v-else>请选中一条消息</p>
          </div>
          <div id="sentimentBlock">
            <h2>情感分析结果</h2>
            <barGraph v-if="returnJSONs[activeJSONIndex]"
              v-bind:sentiment="returnJSONs[activeJSONIndex].sentiment"></barGraph>
            <p v-else>请选中一条消息</p>

          </div>
        </div>
      </section>
    </section>
  </div>

</template>

<script>
import msgBlock from "./components/msgBlock.vue";
import inputBlock from "./components/inputBlock.vue";
import msgBubble from "./components/msgBubble.vue";
import multBarGraph from "./components/multBarGraph.vue";
import barGraph from "./components/barGraph.vue";

export default {
  name: "app",
  data: function() {
    return {
      nextMsgID: 1,
      messages: [],
      returnJSONs: [],
      activeJSONIndex: -1,
      onWhichPage: "left"
    };
  },
  components: {
    msgBlock,
    msgBubble,
    inputBlock,
    multBarGraph,
    barGraph
  },
  methods: {
    appendMsgToMsgBoard: function(halfDoneMsgObj) {
      let IDOfThisMsg = this.nextMsgID;
      halfDoneMsgObj.id = IDOfThisMsg + "_r";
      this.nextMsgID++;

      this.messages.push(halfDoneMsgObj);

      // 向后端发送消息
      var xmlhttp = new XMLHttpRequest();

      xmlhttp.onreadystatechange = function() {
        if (xmlhttp.readyState == 4 && xmlhttp.status == 200) {
          let returnJSON = JSON.parse(xmlhttp.responseText);
          this.returnJSONs.push(returnJSON);

          let returnMsg = {
            whichSide: "left",
            msgContent: returnJSON.reply,
            id: IDOfThisMsg + "_l"
          };
          this.messages.push(returnMsg);
        }
      }.bind(this);

      xmlhttp.open("POST", "http://127.0.0.1:5000/", true);
      xmlhttp.send(halfDoneMsgObj.msgContent);
    },
    setThisMsgActive: function(id) {
      this.activeJSONIndex = id - 1;
    },
    toRightPage: function() {
      this.onWhichPage = "right";
    },
    toLeftPage: function() {
      this.onWhichPage = "left";
    }
  },
  computed: {
    wrapperForTwoPagesClassObj: function() {
      return {
        atRightPage: this.onWhichPage === "right"
      };
    },
    replyFromHuaHua: function() {
      return this.returnJSONs[this.activeJSONIndex]
        ? this.returnJSONs[this.activeJSONIndex].reply
        : "请选中一条消息";
    }
  }
};
</script>

<style>
html,
body {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  margin: 0px;
  padding: 0px;

  overflow-y: hidden;

  background-image: linear-gradient(45deg, #58829b, #13528e);
}

#wrapperForTwoPages {
  display: flex;
  flex-direction: row;

  width: 200%;
  transition: all 0.7s ease-in-out;
}

.page {
  width: 100%;
  height: 100vh;
}

.atRightPage {
  transform: translateX(-50%);
}

#chatWindow {
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

#toAnalyzePage {
  position: absolute;
  top: 0;
  right: 0;

  width: 40px;
  height: 40px;
  background-image: url(./assets/analyze.png);
  background-repeat: round;
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

  box-shadow: 0 -2px 5px rgba(0, 0, 0, 0.16);
}

#analyzePage {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

#msgList {
  color: #2c3e50;
  background-color: #eceff1e5;

  width: 40%;
  height: 80vh;
  box-sizing: border-box;

  padding: 16px;
  padding-top: 76px;
  overflow: auto;
  position: relative;
}

#msgList > div {
  margin-bottom: 8px;
}

#msgList > h2 {
  position: absolute;
  top: 0;
  left: 0;
  height: 60px;
  width: 100%;

  margin: 0;
  padding: 0;

  background-color: #ffffff;
  font-weight: normal;
  text-align: center;
  line-height: 60px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.16);
}

#toChatPage {
  position: absolute;
  top: 8px;
  left: 8px;

  width: 44px;
  height: 44px;
  background-image: url(./assets/msg.png);
  background-repeat: round;
}

#analyzeResults {
  width: 40%;
  height: 80vh;

  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

#replyBlock {
  width: 100%;

  display: flex;
  flex-direction: column;
  align-items: center;
}

#graphBlock {
  width: 100%;

  display: flex;
  justify-content: space-between;
}

#graphBlock > div {
  width: 200px;
  height: 270px;
}

#analyzeResults h2 {
  color: #eceff1;
  font-weight: normal;

  margin: 0;
  padding: 0;
  height: 70px;

  text-align: center;
  line-height: 70px;
}

#sentimentBlock > h2 {
  margin-bottom: 75px;
}
</style>
