<template>
  <div id="wrapperForTwoPages"
    class="atRightPage">
    <section id="chatPage"
      class="page">
      <section id="chatWindow">
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
      </section>
    </section>

    <section id="analyzePage"
      class="page">
      <section id="msgList">
        <div v-for="json in returnJSONs"
          v-bind:key="json.id"
          v-on:click="setThisMsgActive(json.id)">
          <msgBubble v-bind:content="json.message"
            which-side="right"></msgBubble>
        </div>
      </section>
      <section id="analyzeResults">
        <div id="replyBlock">{{returnJSONs[activeJSONIndex].reply}}</div>
        <div id="typeBlock">
          <ringGraph v-bind:types="returnJSONs[activeJSONIndex].types"></ringGraph>
        </div>
        <div id="sentimentBlock">
          <barGraph v-bind:sentiment="returnJSONs[activeJSONIndex].sentiment"></barGraph>
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
          message:
            "《琵琶行》是唐朝诗人白居易的长篇乐府诗之一。作于元和十一年（816年）。此诗通过对琵琶女高超弹奏技艺和她不幸经历的描述，揭露了封建社会官僚腐败、民生凋敝、人才埋没等不合理现象，表达了诗人对她的深切同情，也抒发了诗人对自己无辜被贬的愤懑之情",
          reply: "播放琵琶行",
          sentiment: 0.45,
          types: { 其他: 0.4, 体育: 0.2, 军事: 0.4 }
        }
      ],
      activeJSONIndex: 2
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
    },
    setThisMsgActive: function(id) {
      this.activeJSONIndex = id - 1;
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

  background-image: linear-gradient(45deg, red, blue);
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

#chatPage {
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
  background-color: #eceff1;

  width: 40%;
  height: 80vh;

  overflow: auto;
}

#analyzeResults {
  width: 40%;
  height: 80vh;

  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

#analyzeResults > div {
  width: 100%;
  flex-grow: 1;

  border: 2px solid black;
}
</style>
