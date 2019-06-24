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
          <msgBubble v-bind:content="returnJSONs[activeJSONIndex].reply"
            which-side="left"></msgBubble>
        </div>
        <div id="graphBlock">
          <div id="typeBlock">
            <h2>分类分析结果</h2>
            <ringGraph v-bind:types="returnJSONs[activeJSONIndex].types"></ringGraph>
          </div>
          <div id="sentimentBlock">
            <h2>情感分析结果</h2>
            <barGraph v-bind:sentiment="returnJSONs[activeJSONIndex].sentiment"></barGraph>
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
import ringGraph from "./components/ringGraph.vue";
import barGraph from "./components/barGraph.vue";

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
      ],
      returnJSONs: [
        {
          id: 1,
          message: "浔阳江头夜送客",
          reply: "枫叶荻花秋瑟瑟",
          sentiment: 0.5,
          types: { 其他: 0.25, 美食: 0.25, 宠物: 0.5 }
        },
        {
          id: 2,
          message: "主人下马客在船",
          reply: "举酒欲饮无管弦",
          sentiment: 0.85,
          types: { 其他: 0.3, 体育: 0.3, 军事: 0.4 }
        },
        {
          id: 3,
          message: "醉不成欢惨将别",
          reply: "别时茫茫江浸月",
          sentiment: 0.45,
          types: { 其他: 0.4, 体育: 0.2, 军事: 0.4 }
        },
        {
          id: 4,
          message: "浔阳江头夜送客",
          reply: "枫叶荻花秋瑟瑟",
          sentiment: 0.5,
          types: { 其他: 0.25, 美食: 0.25, 宠物: 0.5 }
        },
        {
          id: 5,
          message: "主人下马客在船",
          reply: "举酒欲饮无管弦",
          sentiment: 0.85,
          types: { 其他: 0.3, 体育: 0.3, 军事: 0.4 }
        },
        {
          id: 6,
          message: "醉不成欢惨将别",
          reply: "别时茫茫江浸月",
          sentiment: 0.45,
          types: { 其他: 0.4, 体育: 0.2, 军事: 0.4 }
        },
        {
          id: 7,
          message: "浔阳江头夜送客",
          reply: "枫叶荻花秋瑟瑟",
          sentiment: 0.5,
          types: { 其他: 0.25, 美食: 0.25, 宠物: 0.5 }
        },
        {
          id: 8,
          message: "主人下马客在船",
          reply: "举酒欲饮无管弦",
          sentiment: 0.85,
          types: { 其他: 0.3, 体育: 0.3, 军事: 0.4 }
        },
        {
          id: 9,
          message: "醉不成欢惨将别",
          reply: "别时茫茫江浸月",
          sentiment: 0.45,
          types: { 其他: 0.4, 体育: 0.2, 军事: 0.4 }
        },
        {
          id: 10,
          message:
            "《琵琶行》是唐朝诗人白居易的长篇乐府诗之一。作于元和十一年（816年）。此诗通过对琵琶女高超弹奏技艺和她不幸经历的描述，揭露了封建社会官僚腐败、民生凋敝、人才埋没等不合理现象，表达了诗人对她的深切同情，也抒发了诗人对自己无辜被贬的愤懑之情",
          reply: "播放琵琶行",
          sentiment: 0.45,
          types: { 其他: 0.4, 体育: 0.2, 军事: 0.4 }
        }
      ],
      activeJSONIndex: 2,
      onWhichPage: "left"
    };
  },
  components: {
    msgBlock,
    msgBubble,
    inputBlock,
    ringGraph,
    barGraph
  },
  methods: {
    appendMsgToMsgBoard: function(halfDoneMsgObj) {
      let lastId = this.messages[this.messages.length - 1].id;
      halfDoneMsgObj.id = lastId + 1;

      this.messages.push(halfDoneMsgObj);

      // 向后端发送消息
      var xmlhttp = new XMLHttpRequest();

      xmlhttp.onreadystatuschange = function() {
        if (xmlhttp.readyState == 4 && xmlhttp.status == 200) {
          let returnJSON = JSON.parse(xmlhttp.responseText);
          this.returnJSONs.push(returnJSON);

          let returnMsg = {
            whichSide: "left",
            msgContent: "reply",
            id: lastId + 2
          };
          this.messages.push(returnMsg);
        }
      };

      xmlhttp.open("POST", "localhost:8080/", true);
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
