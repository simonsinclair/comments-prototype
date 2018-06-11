<template>
  <div class="reply-cta" :class="{ 'reply-cta--replies-visible': isRepliesVisible }">

    <!-- A) Reply, when there are no replies OR -->
    <button
      @click="$emit('reply')"
      v-show="!isRepliesVisible && replies.length === 0">
        <img src="../assets/reply.svg" alt="" /> Reply
    </button>

    <!-- B) Expand replies when there are some. -->
    <button class="gel-pica-bold"
      @click="$emit('show-replies')"
      v-show="!isRepliesVisible && replies.length > 0">
        <img src="../assets/n-replies.svg" alt="" /> {{ replies.length }} Replies
    </button>

    <!-- C) Close reply action/replies. -->
    <button class="gel-pica-bold"
      @click="$emit('hide-replies')"
      v-show="isRepliesVisible">
        <img src="../assets/n-replies.svg" alt="" /> {{ replies.length }} Close
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
  .reply-cta {}
  .reply-cta--replies-visible {

    svg {}
  }
</style>
