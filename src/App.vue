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
  // html {
  //   box-sizing: border-box;
  // }
  // *, *:before, *:after {
  //   box-sizing: inherit;
  // }

  body {
    background: #EEE;
    padding-top: 80px;
    padding-bottom: 200px;
    font-family: "ReithSans", sans-serif;
  }

  button {
    border: none;
    cursor: pointer;
  }
</style>
