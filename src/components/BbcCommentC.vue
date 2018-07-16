<template>
  <div
    class="comment-wrap"
    :class="{ 'comment-wrap--has-replies': hasReplies }">

    <!-- Comment -->
    <div class="comment">
      <div class="comment__header">
        <bbc-contributor :display-name="displayName"></bbc-contributor>
        <!--
          Todo:
            - Render special flag, e.g. Editors pick
        -->
      </div>
      <div class="comment__body">
        <p class="gel-great-primer">{{ commentText }}</p>
        <span class="gel-brevier">
          <span class="comment__timestamp">{{ timestamp | fromNow }}</span>
          <span class="comment__bullet">&bull;</span>
          <span class="comment__report"><a href="#">Report</a></span>
        </span>
      </div>
      <div class="comment__footer">
        <div class="gel-layout">
          <div class="gel-layout__item gel-1/2">
            <bbc-reply-cta-c @reply="doReply()"></bbc-reply-cta-c>
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

    <!-- Comment Replies -->
    <div class="replies" v-show="hasReplies">
      <transition-group name="new-reply" tag="div">
        <bbc-reply
          v-for="reply in getOldestReplies(replies, numVisibleReplies)"
          :key="reply.id"

          :display-name="reply.displayName"
          :reply-text="reply.replyText"
          :timestamp="reply.timestamp"
          :num-up-votes="reply.numUpVotes"
          :num-down-votes="reply.numDownVotes"
        ></bbc-reply>
      </transition-group>
      <button
        v-show="isRepliesLimited"
        class="replies__show-more gel-brevier-bold"
        @click="showMoreReplies()">
          Show more replies
      </button>
      <transition-group name="new-reply" tag="div" class="replies--new">
        <bbc-reply
          v-for="newReply in newReplies"
          :key="newReply.id"

          :display-name="newReply.displayName"
          :reply-text="newReply.replyText"
          :timestamp="newReply.timestamp"
          :num-up-votes="newReply.numUpVotes"
          :num-down-votes="newReply.numDownVotes"
        ></bbc-reply>
      </transition-group>
      <bbc-submit-comment
        class="submit-comment--replies"
        v-show="hasReplies || isReplyFieldVisible"
        ref="submitComment"
        :placeholder-text="'Reply as ' + session.displayName"
        @comment-submitted="submitReply"
        accepts-media=""
        cta-text="Add reply">
      </bbc-submit-comment>
    </div>
  </div>
</template>

<script>
import moment from 'moment';

import BbcReply from './BbcReply';
import BbcReplyCtaC from './BbcReplyCtaC';
import BbcSubmitComment from './BbcSubmitComment';
import BbcContributor from './BbcContributor';

moment.updateLocale('en', {
  relativeTime: {
    s: 'just now',
    m: '1 minute',
    h: '1 hour',
    d: '1 day',
    M: '1 month',
    y: '1 year',
  },
});

export default {
  components: { BbcReply, BbcReplyCtaC, BbcSubmitComment, BbcContributor },

  data() {
    return {
      hasReplies: this.replies.length > 0,
      isReplyFieldVisible: false,
      isRepliesLimited: false,
      numVisibleReplies: 1,
      newReplies: [],
    };
  },

  methods: {
    doReply() {
      this.isReplyFieldVisible = true;
      this.$refs.submitComment.focus();
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

    submitReply(replyText) {
      // If replies are limited, or would be limited after next reply:
      if (this.isRepliesLimited || this.replies.length === this.numVisibleReplies) {
        // Push reply into 'always visible' reply slot.
        this.newReplies.push({
          id: this.newReplies.length + 1, // Increment `nextReplyId` for next reply.
          displayName: this.session.displayName,
          replyText,
          timestamp: new Date(),
          numUpVotes: 0,
          numDownVotes: 0,
        });
      } else {
        // Push reply normally.
        this.replies.push({
          id: this.replies.length + 1, // Increment `nextReplyId` for next reply.
          displayName: this.session.displayName,
          replyText,
          timestamp: new Date(),
          numUpVotes: 0,
          numDownVotes: 0,
        });
      }
    },
  },

  filters: {
    fromNow(timestamp) {
      return moment(timestamp).fromNow(true);
    },
  },

  props: {
    session: Object,
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
          margin-bottom: 0;
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
        background-color: #DDD;
        border-bottom-right-radius: 2px;
        border-bottom-left-radius: 2px;
        box-shadow: 0px 2px 1px 0px rgba(0,0,0,0.3);
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

      // Reply Items
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
