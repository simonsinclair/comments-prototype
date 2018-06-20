<template>
  <div id="app" class="app" :class="themeClass">
    <picture class="app__bg" v-if="theme === 'childrens'">
      <source media="(max-width: 400px)" srcset="./assets/page/childrens/b1_body.png" />
      <source media="(max-width: 600px)" srcset="./assets/page/childrens/b2_body.png" />
      <source media="(max-width: 768px)" srcset="./assets/page/childrens/b3_body.png" />
      <source media="(max-width: 1008px)" srcset="./assets/page/childrens/b4_body.png" />
      <source media="(max-width: 1440px)" srcset="./assets/page/childrens/b5_body.png" />
      <img src="./assets/page/childrens/b6_body.png" alt="" />
    </picture>
    <bbc-comments :session-display-name="sessionDisplayName"></bbc-comments>
    <picture class="app__bg" v-if="theme === 'childrens'">
      <source media="(max-width: 400px)" srcset="./assets/page/childrens/b1_footer.png" />
      <source media="(max-width: 600px)" srcset="./assets/page/childrens/b2_footer.png" />
      <source media="(max-width: 768px)" srcset="./assets/page/childrens/b3_footer.png" />
      <source media="(max-width: 1008px)" srcset="./assets/page/childrens/b4_footer.png" />
      <source media="(max-width: 1440px)" srcset="./assets/page/childrens/b5_footer.png" />
      <img src="./assets/page/childrens/b6_footer.png" alt="" />
    </picture>
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

  .app__bg {

    > img {
      display: block;
      width: 100%;
    }
  }
</style>
