<template>
  <div>
    正面：{{sentiment*100}}；
    负面：{{100 - sentiment*100}}
  </div>
</template>

<script>
export default {
  props: ["sentiment"]
};
</script>

<style scoped>
</style>

