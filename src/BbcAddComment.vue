<template>
  <div class="bbc-comments__add-comment">
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
      commenter: this.displayName,
      commentText: '',
    };
  },
  methods: {
    addComment(e) {
      this.comments.push({
        commenter: this.commenter,
        commentText: this.commentText,
        timestamp: Date.now(),
        numUpVotes: 0,
        numDownVotes: 0,
        replies: [],
      });
      this.commentText = '';

      // eslint-disable-next-line
      console.log(e, this.comments);
    },
  },
};
</script>

<style lang="scss">
  .bbc-comments__add-comment {
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
