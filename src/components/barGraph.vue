<template>
  <div id="barWrapper">
    <span id="leftBar"
      class="bar"
      v-bind:style="leftBarStyleObj">
      <b id="leftLine"
        class="line">
        <span id="leftLabel"
          class="label">负面{{100-this.widthOfRightBar}}%</span>
      </b>
    </span>
    <span id="rightBar"
      class="bar"
      v-bind:style="rightBarStyleObj">
      <b id="rightLine"
        class="line">
        <span id="rightLabel"
          class="label">正面{{this.widthOfRightBar}}%</span>
      </b>
    </span>
    <i id="leftFace"
      class="iconFace"></i>
    <i id="rightFace"
      class="iconFace"></i>
  </div>
</template>

<script>
export default {
  props: ["sentiment"],
  computed: {
    widthOfRightBar: function() {
      return 100 * this.sentiment;
    },
    rightBarStyleObj: function() {
      return {
        width: `${this.widthOfRightBar}%`
      };
    },
    leftBarStyleObj: function() {
      return {
        width: `${100 - this.widthOfRightBar}%`
      };
    }
  }
};
</script>

<style scoped>
#barWrapper {
  position: relative;
}

.bar {
  position: absolute;
  height: 50px;
  top: 0;
  transition: 0.4s cubic-bezier(0.3, 0, 0, 1);
}

#leftBar {
  left: 0;
  background-color: #f9c332;
}

#rightBar {
  right: 0;
  background-color: #1ae15c;
}

.line {
  position: absolute;
  height: 23px;
  border-right: 1px solid #9ebfd8;
}

#leftLine {
  top: -31px;
  right: 20%;
}

#rightLine {
  top: 59px;
  left: 20%;
}

.label {
  font-size: 1em;
  font-style: normal;
  font-weight: 500;
  position: absolute;
  white-space: nowrap;
  color: #9ebfd8;
}

#rightLabel {
  top: 25px;
  left: -25px;
}

#leftLabel {
  top: -25px;
  left: -30px;
}

.iconFace {
  position: absolute;
  top: 0;
  width: 50px;
  height: 50px;
}

#leftFace {
  left: -25px;
  background: url(../assets/bad_face.png) no-repeat 0 0;
}
#rightFace {
  right: -25px;
  background: url(../assets/good_face.png) no-repeat 0 0;
}
</style>

