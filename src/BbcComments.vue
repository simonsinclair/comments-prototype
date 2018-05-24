<template>
  <div class="comments gel-wrap">
    <div class="gel-layout">

      <!-- Title -->
      <div class="gel-layout__item">
        <h1>{{ comments.length }} Comments</h1>
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
          v-for="comment in orderedFilteredAndLimited(comments)"
          :key="comment.id"

          :display-name="comment.displayName"
          :comment-text="comment.commentText"
          :timestamp="comment.timestamp"
          :num-up-votes="comment.numUpVotes"
          :num-down-votes="comment.numDownVotes"
          :replies="comment.replies"
        ></bbc-comment>
      </div>

      <div class="gel-layout__item">
        <button @click="showMoreComments(5)">More Comments</button>
      </div>
    </div>
  </div>
</template>

<script>
import BbcAddComment from './BbcAddComment';
import BbcComment from './BbcComment';

const filters = {
  all(comments) {
    return comments;
  },
};

const orders = {
  oldest(comments) {
    return comments;
  },
  newest(comments) {
    return comments.slice(0).reverse();
  },
};

export default {
  props: {
    displayName: String,
  },

  components: { BbcAddComment, BbcComment },

  data() {
    return {
      activeFilter: 'all',
      activeOrder: 'newest',
      numVisibleComments: 3,
      comments: [
        {
          id: 1,
          displayName: this.displayName,
          commentText: 'First comment!',
          timestamp: Date.now(),
          numUpVotes: 3,
          numDownVotes: 1,
          replies: [],
        },
        {
          id: 2,
          displayName: this.displayName,
          commentText: 'Second comment...',
          timestamp: Date.now(),
          numUpVotes: 2,
          numDownVotes: 0,
          replies: [],
        },
        {
          id: 3,
          displayName: this.displayName,
          commentText: 'Third comment.',
          timestamp: Date.now(),
          numUpVotes: 0,
          numDownVotes: 0,
          replies: [],
        },
      ],
    };
  },

  methods: {
    orderedFilteredAndLimited(rawComments) {
      // 1. Order
      // 2. Filter
      // 3. Limit
      let comments = orders[this.activeOrder](rawComments);
      comments = filters[this.activeFilter](comments);
      comments = comments.slice(0, this.numVisibleComments);
      return comments;
    },

    showMoreComments(numMoreComments) {
      // eslint-disable-next-line
      console.log(numMoreComments);
    },
  },
};
</script>

<style lang="scss" scoped="">
  .comments {
    background-color: #CCC;
  }
</style>
