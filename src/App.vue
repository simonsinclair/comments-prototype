<template>
  <div id="app" class="app" :class="themeClass">
    <div class="app__config" v-show="isConfigVisible">
      <form @submit.prevent="isConfigVisible = false">
        <input type="text" v-model="session.displayName" />
        <fieldset>
          <label>
            A
            <input
              type="radio"
              value="BbcCommentA"
              v-model="session.commentComponent" />
          </label>
          <label>
            B
            <input
              type="radio"
              value="BbcCommentB"
              v-model="session.commentComponent" />
          </label>
          <label>
            C
            <input
              type="radio"
              value="BbcCommentC"
              v-model="session.commentComponent" />
          </label>
        </fieldset>
        <fieldset>
          <label>
            Core
            <input
              type="radio"
              value="core"
              v-model="session.theme" />
          </label>
          <label>
            Children's
            <input
              type="radio"
              value="childrens"
              v-model="session.theme" />
          </label>
        </fieldset>
        <button type="submit">Hide</button>
      </form>
    </div>

    <!-- Head & Body -->
    <picture class="app__bg" v-if="session.theme === 'core'">
      <source media="(max-width: 400px)" srcset="./assets/page/core/b1_body.png" />
      <source media="(max-width: 600px)" srcset="./assets/page/core/b2_body.png" />
      <source media="(max-width: 768px)" srcset="./assets/page/core/b3_body.png" />
      <source media="(max-width: 1008px)" srcset="./assets/page/core/b4_body.png" />
      <source media="(max-width: 1280px)" srcset="./assets/page/core/b5_body.png" />
      <source media="(max-width: 1440px)" srcset="./assets/page/core/b6_body.png" />
      <img src="./assets/page/core/b7_body.png" alt="" />
    </picture>
    <picture class="app__bg" v-if="session.theme === 'childrens'">
      <source media="(max-width: 400px)" srcset="./assets/page/childrens/b1_body.png" />
      <source media="(max-width: 600px)" srcset="./assets/page/childrens/b2_body.png" />
      <source media="(max-width: 768px)" srcset="./assets/page/childrens/b3_body.png" />
      <source media="(max-width: 1008px)" srcset="./assets/page/childrens/b4_body.png" />
      <source media="(max-width: 1440px)" srcset="./assets/page/childrens/b5_body.png" />
      <img src="./assets/page/childrens/b6_body.png" alt="" />
    </picture>

    <!-- Comments -->
    <bbc-comments :session="session"></bbc-comments>

    <!-- Footer -->
    <picture class="app__bg" v-if="session.theme === 'core'">
      <source media="(max-width: 400px)" srcset="./assets/page/core/b1_footer.png" />
      <source media="(max-width: 600px)" srcset="./assets/page/core/b2_footer.png" />
      <source media="(max-width: 768px)" srcset="./assets/page/core/b3_footer.png" />
      <source media="(max-width: 1008px)" srcset="./assets/page/core/b4_footer.png" />
      <source media="(max-width: 1280px)" srcset="./assets/page/core/b5_footer.png" />
      <source media="(max-width: 1440px)" srcset="./assets/page/core/b6_footer.png" />
      <img src="./assets/page/core/b7_footer.png" alt="" />
    </picture>
    <picture class="app__bg" v-if="session.theme === 'childrens'">
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
      isConfigVisible: true,
      session: {
        theme: 'core',
        displayName: 'Alex Jones',
        commentComponent: 'BbcCommentC',
      },
    };
  },
  computed: {
    themeClass() {
      return `app--${this.session.theme}`;
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
    outline: none;
    padding: 0;
  }

  .app__bg {

    > img {
      display: block;
      width: 100%;
    }
  }

  .app__config {
    background-color: rgba(0,0,0,0.75);
    color: #FFF;
    position: fixed;
      top: 8px; left: 8px;
    z-index: 100;

    > form {
      padding: 10px;
      overflow: hidden; // Clearfix.

      > input[type="text"] {
        border: none;
        padding: 6px;
      }

      > fieldset {
        border: 1px solid rgba(255, 255, 255, 0.25);
        margin-left: 0;
        margin-top: 6px;
        margin-right: 0;
        padding: 6px;

        > label {
          margin-right: 8px;
        }
      }

      > button {
        background-color: #3a64ee;
        color: #FFF;
        float: right;
        padding: 4px 8px;
        margin-top: 6px;
        font-weight: bold;
      }
    }
  }
</style>
