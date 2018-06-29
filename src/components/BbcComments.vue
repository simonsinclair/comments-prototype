<template>
  <div class="comments">
    <div class="gel-wrap">
      <div class="gel-layout gel-layout--center">
        <div class="gel-layout__item gel-10/12@m gel-8/12@l">
          <div class="gel-layout">

            <!-- Title -->
            <div class="gel-layout__item">
              <div class="comments__title gel-paragon-bold">
                {{ title }}
              </div>
            </div>

            <!-- Add a Comment -->
            <div class="gel-layout__item">
              <bbc-submit-comment
                class="submit-comment--isolated"
                :placeholder-text="'Comment as ' + session.displayName"
                @comment-submitted="submitComment"
                cta-text="Add comment">
              </bbc-submit-comment>
            </div>

            <!-- Num. Comments -->
            <div class="gel-layout__item">
              <div class="comments__number gel-pica">
                <span>{{ comments.length }}</span> Comments
              </div>
            </div>

            <!-- Sorting/Filtering -->
            <div class="gel-layout__item comments__filter-order">
              <select class="gel-pica-bold" name="showCommentType">
                <option value="all" selected="">Show everything</option>
              </select>

              <select class="gel-pica-bold" name="orderCommentsBy">
                <option value="latest" selected="">Latest first</option>
              </select>
            </div>

            <!-- Comments! -->
            <div class="gel-layout__item">
              <transition-group name="new-comment" tag="div">
                <component
                  :is="session.commentComponent"

                  v-for="comment in
                    getOrderedFilteredAndLimitComments(comments, numVisibleComments)"
                  :key="comment.id"

                  :session="session"
                  :display-name="comment.displayName"
                  :comment-text="comment.commentText"
                  :timestamp="comment.timestamp"
                  :num-up-votes="comment.numUpVotes"
                  :num-down-votes="comment.numDownVotes"
                  :replies="comment.replies">
                </component>
              </transition-group>
            </div>

            <div class="gel-layout__item">
              <button
                class="comments__show-more"
                v-show="isCommentsLimited"
                @click="showMoreComments()">
                  More Comments
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import commentsDataCore from '../comments-core';
// import commentsDataChildrens from '../comments-childrens';

import BbcSubmitComment from './BbcSubmitComment';
import BbcCommentA from './BbcCommentA';
import BbcCommentB from './BbcCommentB';

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
    session: Object,
  },

  components: { BbcSubmitComment, BbcCommentA, BbcCommentB },

  data() {
    return {
      activeFilter: 'all',
      activeOrder: 'newest',
      numVisibleComments: 3,
      isCommentsLimited: false,

      // DATA SCHEMA
      // -----------
      // id: this.nextCommentId += 1, // Increment `nextCommentId` for next comment.
      // displayName: this.session.displayName,
      // commentText: this.commentText,
      // timestamp: new Date(),
      // numUpVotes: 0,
      // numDownVotes: 0,
      // replies: [],

      // To do: data switching based on theme.
      title: commentsDataCore.title,
      comments: commentsDataCore.comments,
    };
  },

  methods: {
    submitComment(commentText) {
      const nextCommentId = this.comments.length + 1;
      this.comments.push({
        id: nextCommentId, // Increment `nextCommentId` for next reply.
        displayName: this.session.displayName,
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

      // Watch/set this elsewhere?
      if (comments.length > this.numVisibleComments) {
        this.isCommentsLimited = true;
      } else {
        this.isCommentsLimited = false;
      }

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
        this.numVisibleComments += 10;
      }
    },
  },
};
</script>

<style lang="scss" scoped="">
  .comments {
    background-color: #EDEDED;
    color: #333;
    padding-top: 64px;
    padding-bottom: 32px-2;
    position: relative;

    .app--childrens & {
      background-color: #363D8D;
    }
  }

    .comments__title {
      margin-bottom: 24px;

      .app--childrens & {
        color: #FFF;
      }

      > span {
        font-weight: bold;
      }
    }

    .comments__filter-order {
      padding-top: 12px;
      padding-bottom: 20px;

      > select {
        border: none;
        outline: none;
        padding: 4px 8px;

        .app--childrens & {
          background-color: #3395D4;
          border-radius: 16px;
        }
      }
    }

    .comments__number {
      margin-top: 24px;

      .app--childrens & {
        color: #FFF;
      }

      > span {
        font-weight: bold;
      }
    }

    .comments__show-more {
      background-color: #FFF;
      border-radius: 4px;
      display: block;
      font-weight: bold;
      margin: 0 auto;
      padding: 12px;
      width: 60%;
      white-space: nowrap;

      .app--childrens & {
        background-color: #3395d4;
        box-shadow: 0px 3px 0px 0px #006db3;
        margin-bottom: 2px;

        &:active {
          margin-top: 2px;
          margin-bottom: 0;
          box-shadow: 0px 1px 0px 0px #006db3;
        }
      }
    }

  // Animation
  //
  .new-comment {}
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
