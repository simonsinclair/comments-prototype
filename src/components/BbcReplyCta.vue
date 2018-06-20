<template>
  <div class="reply-cta" :class="{ 'reply-cta--replies-visible': isRepliesVisible }">

    <!-- A) Reply, when there are no replies. -->
    <button class="gel-pica-bold"
      @click="$emit('reply')"
      v-show="!isRepliesVisible && replies.length === 0">
        <img src="../assets/reply.svg" alt="" /> Reply
    </button>

    <!-- B) Expand reply/replies. -->
    <button class="gel-pica-bold"
      @click="$emit('show-replies')"
      v-show="!isRepliesVisible && replies.length > 0">
        <img src="../assets/reply.svg" alt="" />
        {{ replies.length > 1 ? `${replies.length} Replies` : `${replies.length} Reply` }}
    </button>

    <!-- C) Hide no reply/replies. -->
    <button class="gel-pica-bold"
      @click="$emit('hide-replies')"
      v-show="isRepliesVisible && replies.length === 0">
        <img src="../assets/n-replies.svg" alt="" /> Hide
    </button>

    <!-- D) Hide reply/replies. -->
    <button class="gel-pica-bold"
      @click="$emit('hide-replies')"
      v-show="isRepliesVisible && replies.length > 0">
        <img src="../assets/n-replies.svg" alt="" />
        {{ replies.length > 1 ? `Hide replies` : `Hide reply` }}
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
    display: inline-block;
    border-radius: 4px;

    &:hover {
      background-color: #EEE;
    }
  }

  .reply-cta--replies-visible {
    background-color: #EEE;

    svg {}
  }
</style>
