<template>
  <div class="add-comment">
    <form v-on:submit.prevent="addComment">
      <input type="text" placeholder="Add a comment" v-model="commentText" />
      <button>Media</button>

      <div class="add-comment__controls">
        <button>Cancel</button> <button type="submit">Add comment</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  props: {
    displayName: String,
    comments: Array,
  },

  data() {
    return {
      commentText: '',
      nextCommentId: this.comments.length,
    };
  },

  methods: {
    addComment() {
      // Validate `commentText` or return.
      const commentText = this.commentText && this.commentText.trim();
      if (!commentText) {
        return;
      }

      // eslint-disable-next-line
      console.log('before', this.comments);

      // Post new comment.
      this.comments.push({
        id: this.nextCommentId += 1, // Increment `nextCommentId` for next comment.
        displayName: this.displayName,
        commentText: this.commentText,
        timestamp: Date.now(),
        numUpVotes: 0,
        numDownVotes: 0,
        replies: [],
      });

      // Clear `commentText` after posting.
      this.commentText = '';

      // eslint-disable-next-line
      console.log('after', this.comments);
    },
  },
};
</script>

<style lang="scss" scoped="">
  .add-comment {
    background-color: #FFF;
    padding: 8px;

    input {
      border: none;
      border-bottom: 1px solid;
    }
  }
    .add-comment__controls {
      text-align: right;
    }
</style>
