<template>
  <div class="reply">
    <div class="reply__header">
      <bbc-contributor :display-name="displayName"></bbc-contributor>
    </div>
    <div class="reply__body">
      <p>{{ replyText }}</p>
    </div>
    <div class="reply__footer">
      <button class="gel-pica-bold">Report</button>

      <button class="gel-pica">Up {{ numUpVotes }}</button>
      <button class="gel-pica">Down {{ numDownVotes }}</button>
    </div>
  </div>
</template>

<script>
import BbcContributor from './BbcContributor';

export default {
  components: { BbcContributor },
  props: {
    displayName: String,
    replyText: String,
    numUpVotes: Number,
    numDownVotes: Number,
  },
};
</script>

<style lang="scss" scoped="">
  .reply {
    background-color: #FFF;
    margin-top: 16px;
    margin-left: 20px;
  }
  .reply__header,
  .reply__body {
    padding-right: 12px;
    padding-left: 12px;
  }
  .reply__header {
    padding-top: 12px;
  }
  .reply__footer {

    > button {
      padding: 12px;
    }
  }
</style>

