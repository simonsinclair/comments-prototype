<template>
  <div id="app" class="app">
    <bbc-comments :display-name="displayName"></bbc-comments>
  </div>
</template>

<script>
import BbcComments from './BbcComments';

export default {
  name: 'App',
  components: { BbcComments },
  data() {
    return {
      displayName: 'Bob Ross',
    };
  },
};
</script>

<style lang="scss">
  body {
    background: #EEE;
    padding-top: 72px;
  }

  button {
    cursor: pointer;
  }
</style>
