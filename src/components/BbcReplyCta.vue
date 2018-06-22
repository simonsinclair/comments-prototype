<template>
  <div class="reply-cta" :class="{ 'reply-cta--replies-visible': isRepliesVisible }">

    <!-- A) Reply, when there are no replies. -->
    <button class="gel-pica-bold"
      @click="$emit('reply')"
      v-show="!isRepliesVisible && replies.length === 0">
        <img src="../assets/reply.svg" alt="" /> <span>Reply</span>
    </button>

    <!-- B) Expand reply/replies. -->
    <button class="gel-pica-bold"
      @click="$emit('show-replies')"
      v-show="!isRepliesVisible && replies.length > 0">
        <img src="../assets/reply.svg" alt="" />
        <span>
          {{ replies.length > 1 ? `${replies.length} Replies` : `${replies.length} Reply` }}
        </span>
    </button>

    <!-- C) Hide no reply/replies. -->
    <button class="gel-pica-bold"
      @click="$emit('hide-replies')"
      v-show="isRepliesVisible && replies.length === 0">
        <img src="../assets/n-replies.svg" alt="" /> <span>Hide</span>
    </button>

    <!-- D) Hide reply/replies. -->
    <button class="gel-pica-bold"
      @click="$emit('hide-replies')"
      v-show="isRepliesVisible && replies.length > 0">
        <img src="../assets/n-replies.svg" alt="" />
        <span>{{ replies.length > 1 ? `Hide replies` : `Hide reply` }}</span>
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
      display: flex;

      > img {}

      > span {
        margin-left: 6px;
      }
    }
  }

  .reply-cta--replies-visible {
    background-color: #EEE;
  }
</style>
