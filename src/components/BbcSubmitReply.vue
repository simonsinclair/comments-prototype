<template>
  <div class="submit-comment" :class="{ 'submit-comment--media': acceptsMedia }">
      <form class="submit-comment__form" @submit.prevent="submitComment">
        <input
          ref="input"
          class="gel-great-primer"
          type="text"
          @focus="isActive = true"
          :placeholder="placeholderText"
          v-model.trim="commentText" />
        <img v-if="acceptsMedia" src="../assets/media-chooser.svg" alt="" />
        <div class="submit-comment__controls" v-show="isActive">
          <button type="button" @click="cancelComment()">Cancel</button>
          <button type="submit" v-bind:disabled="!isSubmittable">{{ ctaText }}</button>
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

      this.$emit('reply-submitted', this.commentText);

      // Clear `commentText` after posting.
      this.commentText = '';
      this.isActive = false;
    },

    cancelComment() {
      this.commentText = '';
      this.isActive = false;
      this.$emit('reply-cancelled');
    },
  },

  data() {
    return {
      commentText: '',
      isActive: false,
      isSubmittable: false,
    };
  },

  watch: {
    commentText(newCommentText) {
      // Also test `this.isSubmittable !== true`, so we don't keep setting it.
      if (newCommentText.length > 0 && this.isSubmittable !== true) this.isSubmittable = true;
      if (newCommentText.length < 1) this.isSubmittable = false;
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
  .submit-comment--isolated {
    box-shadow: 0px 2px 1px 0px rgba(0,0,0,0.3)
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

  .submit-comment--replies {
    border-radius: 0;
    margin-right: 12px;
    margin-left: 12px;
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
        &[disabled] {
          background-color: #999;
          cursor: not-allowed;
        }
      }
    }
</style>
