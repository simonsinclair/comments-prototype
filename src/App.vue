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
      displayName: 'Test User',
    };
  },
};
</script>

<style lang="scss">
  body {
    background: #EEE;
    padding-top: 72px;
  }
</style>
