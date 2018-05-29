<template>
  <div class="reply">
    <div class="reply__header">
      <img src="http://placehold.it/24" alt="" />
      <span class="reply__display-name">{{ displayName }}</span>
    </div>
    <div class="reply__body">
      <p>{{ replyText }}</p>
    </div>
    <div class="reply__footer">
      <button>Report</button>

      <button>Up {{ numUpVotes }}</button>
      <button>Down {{ numDownVotes }}</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    displayName: String,
    replyText: String,
    numUpVotes: Number,
    numDownVotes: Number,
  },
};
</script>

<style lang="scss" scoped="">
  .reply {}
</style>

