<template>
  <div
    :id="id"
    class="comment-wrap"
    :class="{ 'comment-wrap--has-replies': replies.length > 0 }">

    <!-- Comment -->
    <div class="comment">
      <div class="comment__header">
        <bbc-contributor-c :display-name="displayName" :timestamp="timestamp"></bbc-contributor-c>
        <!--
          Todo:
            - Render special flag, e.g. Editors pick
        -->
      </div>
      <div class="comment__body">
        <p class="gel-great-primer">{{ commentText }}</p>
        <!-- <span class="gel-brevier">
          <span class="comment__timestamp">{{ timestamp | fromNow }}</span>
          <span class="comment__bullet">&bull;</span>
          <span class="comment__report"><a href="#">Report</a></span>
        </span> -->
      </div>
      <div class="comment__footer">
        <div class="gel-layout">
          <div class="gel-layout__item gel-1/2">
            <bbc-reply-cta-c @reply="preReply()"></bbc-reply-cta-c>
          </div>
          <div class="gel-layout__item gel-1/2 comment__actions">
            <button class="gel-pica">
              <img src="../assets/up-thumb.svg" alt="" /> {{ numUpVotes }}
            </button>
            <button class="gel-pica">
              <img src="../assets/down-thumb.svg" alt="" /> {{ numDownVotes }}
            </button>
            <button><img src="../assets/share.svg" alt="" /></button>
          </div>
        </div>
      </div>
    </div>

    <transition name="new-reply" tag="div">
      <bbc-submit-reply
        v-show="isSubmitReplyVisible"
        ref="submitReply"
        :placeholder-text="'Reply as ' + session.displayName"
        :replying-to-id="id"
        @reply-submitted="submitReply"
        @reply-cancelled="cancelReply"
        cta-text="Add reply">
      </bbc-submit-reply>
    </transition>

    <transition name="new-reply" tag="div">
      <div class="submit-reply-success" v-show="isSubmitReplySuccessVisible">
        <a
            @click="isSubmitReplySuccessVisible = false"
            :href="'#' + latestReplyId">
          View my reply
        </a>
      </div>
    </transition>

    <!-- Comment Replies -->
    <div class="replies" v-show="replies.length > 0">
      <transition-group name="new-reply" tag="div">
        <bbc-reply-c
          v-for="reply in getOldestReplies(replies, numVisibleReplies)"
          :key="reply.id"

          @reply-to-reply-submitted="submitReplyToReply"

          :session="session"

          :id="reply.id"
          :display-name="reply.displayName"
          :reply-to-id="reply.replyToId"
          :reply-text="reply.replyText"
          :timestamp="reply.timestamp"
          :num-up-votes="reply.numUpVotes"
          :num-down-votes="reply.numDownVotes"

          :reply-stack="replies"
          :new-reply-stack="newReplies"
        ></bbc-reply-c>
      </transition-group>
      <button
        v-show="isRepliesLimited"
        class="replies__show-more gel-brevier-bold"
        @click="showMoreReplies()">
          Show more replies
      </button>
      <transition-group name="new-reply" tag="div" class="replies--new">
        <bbc-reply-c
          v-for="newReply in newReplies"
          :key="newReply.id"

          @reply-to-reply-submitted="submitReplyToReply"

          :session="session"

          :id="newReply.id"
          :display-name="newReply.displayName"
          :reply-to-id="newReply.replyToId"
          :reply-text="newReply.replyText"
          :timestamp="newReply.timestamp"
          :num-up-votes="newReply.numUpVotes"
          :num-down-votes="newReply.numDownVotes"

          :reply-stack="replies"
          :new-reply-stack="newReplies"
        ></bbc-reply-c>
      </transition-group>
    </div>
  </div>
</template>

<script>
// import moment from 'moment';
import inView from 'in-view';
import shortid from 'shortid';

import BbcReplyC from './BbcReplyC';
import BbcReplyCtaC from './BbcReplyCtaC';
import BbcSubmitReply from './BbcSubmitReply';
import BbcContributorC from './BbcContributorC';

// moment.updateLocale('en', {
//   relativeTime: {
//     s: 'just now',
//     m: '1 minute',
//     h: '1 hour',
//     d: '1 day',
//     M: '1 month',
//     y: '1 year',
//   },
// });

export default {
  components: { BbcReplyC, BbcReplyCtaC, BbcSubmitReply, BbcContributorC },

  data() {
    return {
      latestReplyId: '',
      isSubmitReplyVisible: false,
      isSubmitReplySuccessVisible: false,
      isRepliesLimited: false,
      numVisibleReplies: 1,
      newReplies: [],
    };
  },

  methods: {
    preReply() {
      this.isSubmitReplySuccessVisible = false;
      this.isSubmitReplyVisible = true;

      this.$nextTick(() => {
        this.$refs.submitReply.focus();
      });
    },

    showMoreReplies() {
      const numTotalReplies = this.replies.length;

      if (numTotalReplies > this.numVisibleReplies) {
        this.numVisibleReplies += 5;
      }
    },

    getOldestReplies(rawReplies, numReplies) {
      // Watch/set this elsewhere?
      if (rawReplies.length > this.numVisibleReplies) {
        this.isRepliesLimited = true;
      } else {
        this.isRepliesLimited = false;
      }
      return rawReplies.slice(0, numReplies);
    },

    submitReply(replyText, replyToId) {
      const newReplyId = shortid.generate();

      // If replies are limited, or would be limited after next reply:
      if (this.isRepliesLimited || this.replies.length === this.numVisibleReplies) {
        // Push reply into 'always visible' reply slot.
        this.newReplies.push({
          id: newReplyId,
          displayName: this.session.displayName,
          replyToId,
          replyText,
          timestamp: new Date(),
          numUpVotes: 0,
          numDownVotes: 0,
        });
      } else {
        // Push reply normally.
        this.replies.push({
          id: newReplyId,
          displayName: this.session.displayName,
          replyToId,
          replyText,
          timestamp: new Date(),
          numUpVotes: 0,
          numDownVotes: 0,
        });
      }

      this.isSubmitReplyVisible = false;

      this.$nextTick(() => {
        this.afterReply(newReplyId);
      });
    },

    afterReply(newReplyId) {
      this.latestReplyId = newReplyId;

      if (!inView.is(document.getElementById(newReplyId))) {
        this.isSubmitReplySuccessVisible = true;
      }
    },

    cancelReply() {
      this.isSubmitReplyVisible = false;
    },

    submitReplyToReply(newReplyId, replyText, replyToId) {
      // If replies are limited, or would be limited after next reply:
      if (this.isRepliesLimited || this.replies.length === this.numVisibleReplies) {
        // Push reply into 'always visible' reply slot.
        this.newReplies.push({
          id: newReplyId,
          displayName: this.session.displayName,
          replyToId,
          replyText,
          timestamp: new Date(),
          numUpVotes: 0,
          numDownVotes: 0,
        });
      } else {
        // Push reply normally.
        this.replies.push({
          id: newReplyId,
          displayName: this.session.displayName,
          replyToId,
          replyText,
          timestamp: new Date(),
          numUpVotes: 0,
          numDownVotes: 0,
        });
      }

      // Because this is a reply to a reply, we don't need
      // to do any 'afterReply()' stuff. The reply component
      // handles that itself.
    },
  },

  // filters: {
  //   fromNow(timestamp) {
  //     return moment(timestamp).fromNow(true);
  //   },
  // },

  props: {
    session: Object,

    id: String,
    displayName: String,
    commentText: String,
    timestamp: Date,
    numUpVotes: Number,
    numDownVotes: Number,
    replies: Array,
  },
};
</script>

<style lang="scss" scoped="">
  .comment-wrap {

      &:last-child {
        margin-bottom: 24px;
      }

      &.comment-wrap--has-replies {}

      &.comment-wrap--replies-visible {
        margin-bottom: 24px;
      }
    }

      .comment {
        background-color: #FFF;
        border-radius: 2px;
        box-shadow: 0px 2px 1px 0px rgba(0,0,0,0.3);
        margin-bottom: 16px;
        position: relative;

        .comment-wrap--has-replies & {
          margin-bottom: 16px+8;

          &::before,
          &::after {
            background-color: #DDD;
            border-radius: 0 0 2px 2px;
            box-shadow: 0 1px 1px rgba(0,0,0,0.3), inset 0 1px 1px rgba(0,0,0,0.3);
            content: '';
            display: block;
            height: 4px;
            position: absolute;
          }

          &::before {
            right: 8px;
            bottom: -4px;
            left: 8px;
            // width: 94%;
          }

          &::after {
            background-color: #CCC;
            right: 16px;
            bottom: -8px;
            left: 16px;
            // width: 88%;
          }
        }

        .comment-wrap--replies-visible & {
          border-bottom-right-radius: 0;
          border-bottom-left-radius: 0;
          margin-bottom: 0;

          &::before,
          &::after {
            content: normal;
          }
        }
      }
        .comment__header,
        .comment__body {
          padding-right: 12px;
          padding-left: 12px;
        }
        .comment__header {
          padding-top: 12px;
        }
          .comment__display-name {}
        .comment__body {

          > p {
            white-space: pre-line;
          }
        }
          .comment__timestamp {}
          .comment__bullet {
            margin-left: 4px;
          }
          .comment__report {
            margin-left: 4px;
          }
        .comment__footer {
          margin-top: 12px;
          padding-right: 12px-4;
          padding-bottom: 12px;
          padding-left: 12px-4;
        }
          .comment__actions {
            text-align: right;

            button {
              padding: 4px 8px;
            }
          }


      // REPLIES
      //

      .replies {
        // background-color: #DDD;
        border-bottom-right-radius: 2px;
        border-bottom-left-radius: 2px;
        // box-shadow: 0px 2px 1px 0px rgba(0,0,0,0.3);
        margin-bottom: 24px;
        padding-top: 12px;
        padding-bottom: 12px;
      }

      .replies__show-more {
        color: #3a64ee;
        display: block;
        margin-right: auto;
        margin-left: auto;
        padding: 4px 16px 16px 16px;
      }


      // Animation
      //

      // Replies
      //

      .new-reply {}

      // Enter
      .new-reply-enter {
        opacity: 0;
        transform: translateY(-32px);
      }
      .new-reply-enter-active {
        transition: all 350ms ease-out;
      }
      .new-reply-enter-to {}

      // Leave
      .new-reply-leave {}
      .new-reply-leave-active {}
      .new-reply-leave-to {}
</style>
