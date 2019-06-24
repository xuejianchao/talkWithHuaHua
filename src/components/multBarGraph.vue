<template>
  <div id="wrapper">
    <div id="firstBar"
      class="bar"
      v-bind:style="firstBarstyleObj">
      <span class="label valueLabel">{{initedTypes[0].type}} {{initedTypes[0].value}}</span>
    </div>

    <div id="secondBar"
      class="bar"
      v-bind:style="secondBarstyleObj">
      <span class="label valueLabel">{{initedTypes[1].type}} {{initedTypes[1].value}}</span>
    </div>
    <div id="thirdBar"
      class="bar"
      v-bind:style="thirdBarstyleObj">
      <span class="label valueLabel">{{initedTypes[2].type}} {{initedTypes[2].value}}</span>
    </div>
  </div>
</template>

<script>
export default {
  props: ["types"],
  data: function() {
    return {};
  },
  computed: {
    initedTypes: function() {
      return this.types;
    },
    firstBarstyleObj: function() {
      return {
        width: `${this.initedTypes[0].value * 100}%`
      };
    },
    secondBarstyleObj: function() {
      return {
        width: `${this.initedTypes[1].value * 100}%`
      };
    },
    thirdBarstyleObj: function() {
      return {
        width: `${this.initedTypes[2].value * 100}%`
      };
    }
  }
};
</script>

<style scoped>
#wrapper {
  width: 180px;
  height: 200px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}

.bar {
  height: 40px;

  position: relative;

  transition: 0.4s cubic-bezier(0.3, 0, 0, 1);
}

#firstBar {
  background-color: rgb(249, 195, 50);
}

#secondBar {
  background-color: rgb(40, 125, 231);
}

#thirdBar {
  background-color: rgb(29, 224, 95);
}

.label {
  line-height: 40px;
  position: absolute;
  white-space: nowrap;
}

.typeLabel {
  color: #000;
  right: 4px;
}

.valueLabel {
  color: #ffffff;
  left: 102%;
}
</style>
