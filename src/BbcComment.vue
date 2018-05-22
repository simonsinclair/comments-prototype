<template>
  <div class="bbc-comments__comment">

    <!-- Comment -->
    <div class="comment__header">
      <img src="http://placehold.it/32" alt="" />
      <span class="comment__commenter">{{ commenter }}</span>
      <!--
        Todo:
          - Render special flag, e.g. Editors pick
      -->
    </div>
    <div class="comment__body">
      <p>{{ commentText }}</p>
      <span class="comment__timestamp">{{ timestamp }}</span>
    </div>
    <div class="comment__footer">
      <button>{{ replies.length }} Replies</button>

      <button>Report</button>
      <button>Up {{ numUpVotes }}</button>
      <button>Down {{ numDownVotes }}</button>
    </div>

    <!-- Replies -->
    <div class="comment__replies">
      <!-- Render replies and stack, here -->
    </div>
  </div>
</template>

<script>
export default {
  props: {
    commenter: String,
    commentText: String,
    timestamp: Number,
    numUpVotes: Number,
    numDownVotes: Number,
    replies: Array,
  },
};
</script>

<style lang="scss">
  .bbc-comments__comment {
    background-color: #FFF;
    padding: 8px;
    margin-bottom: 16px;
  }
    .comment__commenter {

    }
</style>
