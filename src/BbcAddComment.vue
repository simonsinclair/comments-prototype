<template>
  <div class="add-comment">
    <form @submit.prevent="addComment">
      <input type="text" placeholder="Add a comment" v-model.trim="commentText" />
      <button>Media</button>

      <div class="add-comment__controls" v-show="showSubmit">
        <button @click="cancelComment()">Cancel</button> <button type="submit">Add comment</button>
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
      showSubmit: false,
    };
  },

  watch: {
    commentText(newCommentText) {
      if (newCommentText.length > 0) {
        this.showSubmit = true;
      } else {
        this.showSubmit = false;
      }
    },
  },

  methods: {
    addComment() {
      if (!this.commentText) {
        return;
      }

      // Post new comment.
      this.comments.push({
        id: this.nextCommentId += 1, // Increment `nextCommentId` for next comment.
        displayName: this.displayName,
        commentText: this.commentText,
        timestamp: new Date(),
        numUpVotes: 0,
        numDownVotes: 0,
        replies: [],
      });

      // Clear `commentText` after posting.
      this.commentText = '';
    },

    cancelComment() {
      this.commentText = '';
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
