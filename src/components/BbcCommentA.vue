<template>
  <div class="comment-wrap" :class="{ 'comment-wrap--replies-visible': isRepliesVisible }">

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
          <div class="gel-layout__item gel-1/3">
            <bbc-reply-cta
              :replies="replies"
              :is-replies-visible="isRepliesVisible"
              @reply="doReply()"
              @show-replies="isRepliesVisible = true"
              @hide-replies="isRepliesVisible = false">
            </bbc-reply-cta>
          </div>
          <div class="gel-layout__item gel-2/3 comment__actions">
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
    <div class="replies" v-show="isRepliesVisible">
      <button
        v-show="isRepliesLimited"
        class="replies__show-earlier"
        @click="showEarlierReplies()">
          Show older replies
      </button>
      <transition-group name="new-reply" tag="div">
        <bbc-reply
          v-for="reply in getLatestReplies(replies, numVisibleReplies)"
          :key="reply.id"

          :display-name="reply.displayName"
          :reply-text="reply.replyText"
          :timestamp="reply.timestamp"
          :num-up-votes="reply.numUpVotes"
          :num-down-votes="reply.numDownVotes"
        ></bbc-reply>
      </transition-group>
      <bbc-submit-comment
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
import BbcReplyCta from './BbcReplyCta';
import BbcSubmitComment from './BbcSubmitComment';
import BbcContributor from './BbcContributor';

moment.updateLocale('en', {
  relativeTime: {
    s: 'just now',
  },
});

export default {
  components: { BbcReply, BbcReplyCta, BbcSubmitComment, BbcContributor },

  data() {
    return {
      isRepliesVisible: false,
      isRepliesLimited: false,
      numVisibleReplies: 5,
    };
  },

  methods: {
    doReply() {
      this.isRepliesVisible = true;
      this.$refs.submitComment.focus();
    },

    showEarlierReplies() {
      const numTotalReplies = this.replies.length;

      if (numTotalReplies > this.numVisibleReplies) {
        this.numVisibleReplies += 5;
      }
    },

    getLatestReplies(rawReplies, numReplies) {
      // Watch/set this elsewhere?
      if (rawReplies.length > this.numVisibleReplies) {
        this.isRepliesLimited = true;
      } else {
        this.isRepliesLimited = false;
      }
      return rawReplies.slice(-numReplies);
    },

    submitReply(replyText) {
      const nextReplyId = this.replies.length + 1;
      this.replies.push({
        id: nextReplyId, // Increment `nextReplyId` for next reply.
        displayName: this.session.displayName,
        replyText,
        timestamp: new Date(),
        numUpVotes: 0,
        numDownVotes: 0,
      });
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

    &.comment-wrap--replies-visible {
      margin-bottom: 32px;
    }
  }

    .comment {
      background-color: #FFF;
      border-radius: 4px;
      margin-bottom: 20px;
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

        button {
          padding: 12px;
        }
      }
        .comment__actions {
          text-align: right;
        }

    .replies {
      margin-right: 8px;
      margin-bottom: 24px;
      margin-left: 8px;
    }

    .replies__show-earlier,
    .replies__show-new {
      background-color: #EEE;
      display: block;
      margin: 0 auto 20px;
      padding: 8px 16px;
    }

    .replies__show-earlier {
      border-radius: 17px;
    }

    .replies__show-new {}

    // Animation
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
