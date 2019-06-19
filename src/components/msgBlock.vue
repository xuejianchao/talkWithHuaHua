<template>
  <div class="msgBlock"
    v-bind:class="msgBlockClassObj">
    <div class="profile"
      v-if="msg.whichSide === 'left'">华华</div>
    <msgBubble v-bind:which-side="msg.whichSide"
      v-bind:content="msg.msgContent"></msgBubble>
  </div>
</template>

<script>
import msgBubble from "./msgBubble.vue";

export default {
  data: function() {
    return {};
  },
  computed: {
    msgBlockClassObj: function() {
      return {
        baseRight: this.msg.whichSide === "right"
      };
    }
  },
  props: ["msg"],
  components: { msgBubble }
};
</script>

<style>
.msgBlock {
  margin-bottom: 8px;
  display: flex;
}

.profile {
  width: 32px;
  height: 32px;
  border-radius: 16px;

  margin-right: 8px;

  color: white;
  background-color: #1296db;

  display: flex;
  align-items: center;
  justify-content: center;
}

.baseRight {
  flex-direction: row-reverse;
}
</style>


