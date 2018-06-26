<template>
  <div class="reply-cta" :class="{ 'reply-cta--replies-visible': isRepliesVisible }">

    <!-- A) Reply, when there are no replies. -->
    <button class="gel-pica-bold"
      @click="$emit('reply')"
      v-show="!isRepliesVisible && replies.length === 0">
        <div>
          <img src="../assets/reply.svg" alt="" /> <span>Reply</span>
        </div>
    </button>

    <!-- B) Expand reply/replies. -->
    <button class="gel-pica-bold"
      @click="$emit('show-replies')"
      v-show="!isRepliesVisible && replies.length > 0">
        <div>
          <img src="../assets/reply.svg" alt="" />
          <span>
            {{ replies.length > 1 ? `${replies.length} Replies` : `${replies.length} Reply` }}
          </span>
        </div>
    </button>

    <!-- C) Hide no reply/replies. -->
    <button class="gel-pica-bold"
      @click="$emit('hide-replies')"
      v-show="isRepliesVisible && replies.length === 0">
        <div>
          <img src="../assets/n-replies.svg" alt="" /> <span>Hide</span>
        </div>
    </button>

    <!-- D) Hide reply/replies. -->
    <button class="gel-pica-bold"
      @click="$emit('hide-replies')"
      v-show="isRepliesVisible && replies.length > 0">
        <div>
          <img src="../assets/n-replies.svg" alt="" />
          <span>{{ replies.length > 1 ? `Hide replies` : `Hide reply` }}</span>
        </div>
    </button>
  </div>
</template>

<script>
export default {
  props: {
    isRepliesVisible: Boolean,
    replies: Array,
  },
};
</script>

<style lang="scss" scoped="">
  .reply-cta {
    border-radius: 4px;
    display: inline-block;
    padding: 4px;

    &:hover {
      background-color: #EEE;
    }

    > button {

      > div {
        display: flex;

        > img {}

        > span {
          margin-left: 6px;
        }
      }
    }
  }

  .reply-cta--replies-visible {
    background-color: #EEE;
  }
</style>
