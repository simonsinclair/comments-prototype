<template>
  <div class="reply gel-layout">
    <div class="gel-layout__item">
      <p>{{ replyText }}</p>
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
