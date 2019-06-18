<template>
  <div v-bind:class="classObject">
    {{content}}
  </div>
</template>

<script>
export default {
  data: function() {
    return {};
  },
  props: ["whichSide", "content"],
  computed: {
    classObject: function() {
      return {
        msgBubble: true,
        leftSide: this.whichSide === "left",
        rightSide: this.whichSide === "right"
      };
    }
  }
};
</script>

<style>
.msgBubble {
  padding: 0.8rem;
  border-radius: 0.5rem;
  width: fit-content;
}

.leftSide {
  background-color: #ffffff;
  border-radius: 0 0.5rem 0.5rem 0.5rem;
}

.rightSide {
  background-color: #cfd8dc;
  border-radius: 0.5rem 0 0.5rem 0.5rem;
}
</style>

