<template>
  <div class="reply-cta">
    <button class="gel-pica-bold" @click="$emit('reply', replyTo)">
      <div>
        <img src="../assets/reply.svg" alt="" /> <span>Reply</span>
      </div>
    </button>
  </div>
</template>

<script>
export default {
  props: {
    replyTo: String,
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
</style>
