<template>
  <div class="submit-comment" :class="{ 'submit-comment--media': acceptsMedia }">
      <form class="submit-comment__form" @submit.prevent="submitComment">
        <input
          ref="input"
          class="gel-great-primer"
          type="text"
          :placeholder="placeholderText"
          v-model.trim="commentText" />
        <img v-if="acceptsMedia" src="../assets/media-chooser.svg" alt="" />
        <div class="submit-comment__controls" v-show="showSubmit">
          <button type="button" @click="cancelComment()">Cancel</button>
          <button type="submit">{{ ctaText }}</button>
        </div>
      </form>
  </div>
</template>

<script>
export default {
  props: {
    acceptsMedia: {
      type: Boolean,
      default: false,
    },
    ctaText: String,
    placeholderText: String,
  },

  methods: {
    focus() {
      this.$nextTick(() => {
        this.$refs.input.focus();
      });
    },

    submitComment() {
      if (!this.commentText) {
        return;
      }

      this.$emit('comment-submitted', this.commentText);

      // Clear `commentText` after posting.
      this.commentText = '';
    },

    cancelComment() {
      this.commentText = '';
    },
  },

  data() {
    return {
      commentText: '',
      showSubmit: false,
    };
  },

  watch: {
    commentText(newCommentText) {
      // Also test `this.showSubmit !== true`, so we don't keep setting it.
      if (newCommentText.length > 0 && this.showSubmit !== true) this.showSubmit = true;
      if (newCommentText.length < 1) this.showSubmit = false;
    },
  },
};
</script>

<style lang="scss" scoped="">
  .submit-comment {
    background-color: #FFF;
    border-radius: 4px;
    padding: 12px;
  }
    .submit-comment__form {

      > input {
        border: none;
        border-bottom: 2px solid #CCC;
        margin-bottom: 12px;
        padding: 0;
        padding-bottom: 8px;
        width: 100%;
      }
    }

  .submit-comment--media {

    .submit-comment__form {
      position: relative;
      padding-right: 34px + 12;

      > img {
        position: absolute;
          top: 0; right: 0;
      }
    }
  }

    .submit-comment__controls {
      text-align: right;

      > button {
        padding: 12px;

        &[type="submit"] {
          background-color: #3a64ee;
          color: #FFF;
        }
      }
    }
</style>
