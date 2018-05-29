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
          v-for="comment in getOrderedFilteredAndLimitComments(comments, numVisibleComments)"
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
        <button class="comments__show-more" @click="showMoreComments">More Comments</button>
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
          // new Date(year, monthIndex [, day [, hours [, minutes [, seconds [, milliseconds]]]]]);
          // The argument monthIndex is 0-based. This means that January = 0 and December = 11.
          timestamp: new Date(2018, 4, 23),
          numUpVotes: 3,
          numDownVotes: 1,
          replies: [
            {
              displayName: this.displayName,
              replyText: 'First reply.',
              numUpVotes: 1,
              numDownVotes: 0,
            },
            {
              displayName: this.displayName,
              replyText: 'Second reply.',
              numUpVotes: 2,
              numDownVotes: 1,
            },
          ],
        },
        {
          id: 2,
          displayName: this.displayName,
          commentText: 'Second comment...',
          timestamp: new Date(2018, 4, 23),
          numUpVotes: 2,
          numDownVotes: 0,
          replies: [],
        },
        {
          id: 3,
          displayName: this.displayName,
          commentText: 'Third comment.',
          timestamp: new Date(2018, 4, 23),
          numUpVotes: 0,
          numDownVotes: 0,
          replies: [],
        },
      ],
    };
  },

  methods: {
    getOrderedAndFilteredComments(rawComments) {
      // 1. Order
      // 2. Filter
      let comments = orders[this.activeOrder](rawComments);
      comments = filters[this.activeFilter](comments);
      return comments;
    },

    getOrderedFilteredAndLimitComments(rawComments, numComments) {
      const comments = this.getOrderedAndFilteredComments(rawComments);
      return comments.slice(0, numComments);
    },

    showMoreComments() {
      const numTotalComments = this.getOrderedFilteredAndLimitComments(this.comments).length;

      if (numTotalComments > this.numVisibleComments) {
        this.numVisibleComments += 5;
      }
    },
  },
};
</script>

<style lang="scss" scoped="">
  .comments {
    background-color: #CCC;
  }
  .comments__show-more {
    background-color: #FFF;
    font-weight: bold;
    padding: 12px 32px;
  }
</style>
