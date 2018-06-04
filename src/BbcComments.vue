<template>
  <div class="comments">
    <div class="gel-wrap">
      <div class="gel-layout gel-layout--center">
        <div class="gel-layout__item gel-10/12@m gel-8/12@l">
          <div class="gel-layout">

            <!-- Title -->
            <div class="gel-layout__item">
              <span class="comments__title gel-paragon">
                <span>{{ comments.length }}</span> Comments
              </span>
            </div>

            <!-- Add a Comment -->
            <div class="gel-layout__item">
              <bbc-submit-comment
                :display-name="displayName"
                @comment-submitted="submitComment"
                placeholder-text="Add a comment"
                cta-text="Add comment">
              </bbc-submit-comment>
            </div>

            <!-- Sorting/Filtering -->
            <div class="gel-layout__item comments__filter-order">
              <select name="showCommentType">
                <option value="all" selected="">Everything</option>
              </select>

              <select name="orderCommentsBy">
                <option value="latest" selected="">Latest first</option>
              </select>
            </div>

            <!-- Comments! -->
            <div class="gel-layout__item">
              <transition-group name="new-comment" tag="div">
                <bbc-comment
                  v-for="comment in
                    getOrderedFilteredAndLimitComments(comments, numVisibleComments)"
                  :key="comment.id"

                  :display-name="comment.displayName"
                  :comment-text="comment.commentText"
                  :timestamp="comment.timestamp"
                  :num-up-votes="comment.numUpVotes"
                  :num-down-votes="comment.numDownVotes"
                  :replies="comment.replies"
                ></bbc-comment>
              </transition-group>
            </div>

            <button class="comments__show-more" @click="showMoreComments">More Comments</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import BbcSubmitComment from './BbcSubmitComment';
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

  components: { BbcSubmitComment, BbcComment },

  data() {
    return {
      activeFilter: 'all',
      activeOrder: 'newest',
      numVisibleComments: 2,

      // DATA SCHEMA
      // -----------
      // id: this.nextCommentId += 1, // Increment `nextCommentId` for next comment.
      // displayName: this.displayName,
      // commentText: this.commentText,
      // timestamp: new Date(),
      // numUpVotes: 0,
      // numDownVotes: 0,
      // replies: [],

      comments: [
        {
          id: 1,
          displayName: this.displayName,
          commentText: 'There comes a nice little fluffer. Only eight colours that you need.',
          // new Date(year, monthIndex [, day [, hours [, minutes [, seconds [, milliseconds]]]]]);
          // The argument monthIndex is 0-based. This means that January = 0 and December = 11.
          timestamp: new Date(2018, 4, 23),
          numUpVotes: 3,
          numDownVotes: 1,
          replies: [
            {
              id: 1,
              displayName: this.displayName,
              replyText: 'Anyone can paint.',
              timestamp: new Date(2018, 4, 23),
              numUpVotes: 1,
              numDownVotes: 0,
            },
            {
              id: 2,
              displayName: this.displayName,
              replyText: 'Maybe we got a few little happy bushes here, just covered with snow. The man who does the best job is the one who is happy at his job.',
              timestamp: new Date(2018, 4, 23),
              numUpVotes: 2,
              numDownVotes: 1,
            },
          ],
        },
        {
          id: 2,
          displayName: this.displayName,
          commentText: 'For the lack of a better word I call them hangy downs. Van Dyke Brown is a very nice brown, it\'s almost like a chocolate brown.',
          timestamp: new Date(2018, 4, 23),
          numUpVotes: 2,
          numDownVotes: 0,
          replies: [],
        },
        {
          id: 3,
          displayName: this.displayName,
          commentText: 'But we\'re not there yet, so we don\'t need to worry about it. You can\'t have light without dark. You can\'t know happiness unless you\'ve known sorrow.',
          timestamp: new Date(2018, 4, 20),
          numUpVotes: 0,
          numDownVotes: 0,
          replies: [],
        },
      ],
    };
  },

  methods: {
    submitComment(commentText) {
      const nextCommentId = this.comments.length + 1;
      this.comments.push({
        id: nextCommentId, // Increment `nextCommentId` for next reply.
        displayName: this.displayName,
        commentText,
        timestamp: new Date(),
        numUpVotes: 0,
        numDownVotes: 0,
        replies: [],
      });
    },

    getOrderedAndFilteredComments(rawComments) {
      // 1. Order
      // 2. Filter
      let comments = orders[this.activeOrder](rawComments);
      comments = filters[this.activeFilter](comments);
      return comments;
    },

    getOrderedFilteredAndLimitComments(rawComments, numComments) {
      if (typeof numComments !== 'number') {
        throw Error('getOrderedFilteredAndLimitComments should be passed a Number as the second argument.');
      }
      const comments = this.getOrderedAndFilteredComments(rawComments);
      return comments.slice(0, numComments);
    },

    showMoreComments() {
      const numTotalComments = this.getOrderedAndFilteredComments(this.comments).length;

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
    padding-top: 32px;
    padding-bottom: 28px;
    position: relative;
  }
    .comments__title {
      display: block;

      > span {
        font-weight: bold;
      }
    }

    .comments__filter-order {
      padding-top: 20px;
      padding-bottom: 20px;
    }

    .comments__show-more {
      background-color: #FFF;
      font-weight: bold;
      padding: 12px 32px;
      position: absolute;
        bottom: 0; left: 50%;
      transform: translate(-50%, 50%);
      white-space: nowrap;
    }

  // Animation
  //
  .new-comment {}

  // .new-comment-enter-active, .new-comment-leave-active {
  //   transition: all 1s;
  // }
  // .new-comment-enter
  //   opacity: 0;
  //   transform: translateY(-32px);
  // }

  .new-comment-leave-to {
    transform: translateY(32px);
  }
  // Enter
  .new-comment-enter {
    opacity: 0;
    transform: translateY(-32px);
  }
  .new-comment-enter-active {
    transition: all 350ms ease-out;
  }
  .new-comment-enter-to {}

  // Leave
  .new-comment-leave {}
  .new-comment-leave-active {}
  .new-comment-leave-to {}
</style>
