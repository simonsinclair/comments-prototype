<template>
  <div id="app" class="app" :class="themeClass">
    <bbc-comments :session-display-name="sessionDisplayName"></bbc-comments>
  </div>
</template>

<script>
import BbcComments from './components/BbcComments';

export default {
  name: 'App',
  components: { BbcComments },
  data() {
    return {
      theme: 'childrens',
      sessionDisplayName: 'Bob Ross',
    };
  },
  computed: {
    themeClass() {
      return `app--${this.theme}`;
    },
  },
};
</script>

<style lang="scss">
  html {
    box-sizing: border-box;
  }
  *, *:before, *:after {
    box-sizing: inherit;
  }

  $blue: #3a64ee;

  body {
    padding-top: 80px;
    padding-bottom: 200px;
    font-family: "ReithSans", sans-serif;
  }

  a {
    color: inherit;
    font-weight: bold;
  }

  button {
    background: none;
    border: none;
    cursor: pointer;
  }
</style>
