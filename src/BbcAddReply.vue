<template>
  <div class="add-reply">
    <form v-on:submit.prevent="addReply">
      <input type="text" placeholder="Add your reply" v-model="replyText" />
      <button>Media</button>

      <div class="add-reply__controls">
        <button>Cancel</button> <button type="submit">Add reply</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  props: {
    displayName: String,
    replies: Array,
  },

  data() {
    return {
      replyText: '',
      nextReplyId: this.replies.length,
    };
  },

  methods: {
    addReply() {
      // Validate `replyText` or return.
      const replyText = this.replyText && this.replyText.trim();
      if (!replyText) {
        return;
      }

      // Post new reply.
      this.replies.push({
        id: this.nextReplyId += 1, // Increment `nextReplyId` for next reply.
        displayName: this.displayName,
        replyText: this.replyText,
        timestamp: new Date(),
        numUpVotes: 0,
        numDownVotes: 0,
      });

      // Clear `replyText` after posting.
      this.replyText = '';
    },
  },
};
</script>

<style lang="scss" scoped="">
  .add-reply {
    background-color: #FFF;
    padding: 8px;

    input {
      border: none;
      border-bottom: 1px solid;
    }
  }
    .add-reply__controls {
      text-align: right;
    }
</style>
