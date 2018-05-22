<template>
  <div class="bbc-comments gel-wrap">
    <div class="gel-layout">

      <!-- Title -->
      <div class="gel-layout__item">
        <h1>{{ comments.length }} Comments {{ displayName }}</h1>
      </div>

      <!-- Add a Comment -->
      <div class="gel-layout__item">
        <bbc-add-comment :display-name="displayName" :comments="comments"></bbc-add-comment>
      </div>

      <!-- Sorting/Filtering -->
      <div class="gel-layout__item">
        <select name="showCommentType">
          <option value="all" selected="">Everything</option>
        </select>

        <select name="orderCommentsBy">
          <option value="latest" selected="">Latest first</option>
        </select>
      </div>

      <!-- Comments! -->
      <div class="gel-layout__item">
        <bbc-comment
          v-for="comment in comments"
          :key="comment.id"

          :commenter="comment.commenter"
          :comment-text="comment.commentText"
          :timestamp="comment.timestamp"
          :num-up-votes="comment.numUpVotes"
          :num-down-votes="comment.numDownVotes"
          :replies="comment.replies"
        ></bbc-comment>
      </div>

    </div>
  </div>
</template>

<script>
import BbcAddComment from './BbcAddComment';
import BbcComment from './BbcComment';

export default {
  props: {
    displayName: String,
  },
  components: { BbcAddComment, BbcComment },
  data() {
    return {
      comments: [
        {
          id: 0,
          commenter: this.displayName,
          commentText: 'An initial comment.',
          timestamp: Date.now(),
          numUpVotes: 3,
          numDownVotes: 1,
          replies: [],
        },
      ],
    };
  },
};
</script>

<style lang="scss">
  .bbc-comments {
    background-color: #CCC;
  }
</style>
