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
          <div class="gel-layout__item gel-1/2">
            <button class="gel-pica-bold comment__reply-btn"
              @click="toggleReplies()"
              v-show="!isRepliesVisible">
              <span v-show="replies.length > 0">
                <img src="./assets/n-replies.svg" alt="" /> {{ replies.length }} Replies
              </span>
              <span v-show="replies.length === 0">
                <img src="./assets/reply.svg" alt="" /> Reply
              </span>
            </button>
            <button class="gel-pica-bold comment__reply-btn"
              @click="toggleReplies()"
              v-show="isRepliesVisible">
              <img src="./assets/n-replies.svg" alt="" /> {{ replies.length }} Close
            </button>
          </div>
          <div class="gel-layout__item gel-1/2">
            <button class="gel-pica">
              <img src="./assets/up-thumb.svg" alt="" /> {{ numUpVotes }}
            </button>
            <button class="gel-pica">
              <img src="./assets/down-thumb.svg" alt="" /> {{ numDownVotes }}
            </button>
            <button><img src="./assets/share.svg" alt="" /></button>
          </div>
        </div>
      </div>
    </div>

    <!-- Comment Replies -->
    <div class="replies" v-show="isRepliesVisible">
      <transition-group name="new-reply" tag="div">
        <bbc-reply
          v-for="reply in replies"
          :key="reply.id"

          :display-name="reply.displayName"
          :reply-text="reply.replyText"
          :timestamp="reply.timestamp"
          :num-up-votes="reply.numUpVotes"
          :num-down-votes="reply.numDownVotes"
        ></bbc-reply>
      </transition-group>
      <bbc-submit-comment
        :display-name="displayName"
        :placeholder-text="'Reply as ' + displayName"
        @comment-submitted="submitReply"
        accepts-media=""
        cta-text="Add reply">
      </bbc-submit-comment>
    </div>
  </div>
</template>

<script>
import moment from 'moment';

moment.updateLocale('en', {
    relativeTime : {
        // future: "in %s",
        // past:   "%s ago",
        s  : 'just now',
        // ss : '%d seconds',
        // m:  "a minute",
        // mm: "%d minutes",
        // h:  "an hour",
        // hh: "%d hours",
        // d:  "a day",
        // dd: "%d days",
        // M:  "a month",
        // MM: "%d months",
        // y:  "a year",
        // yy: "%d years"
    },
});

import BbcReply from './BbcReply';
import BbcSubmitComment from './BbcSubmitComment';
import BbcContributor from './BbcContributor';

export default {
  components: { BbcReply, BbcSubmitComment, BbcContributor },

  data() {
    return {
      isRepliesVisible: false,
    };
  },

  methods: {
    submitReply(replyText) {
      const nextReplyId = this.replies.length + 1;
      this.replies.push({
        id: nextReplyId, // Increment `nextReplyId` for next reply.
        displayName: this.displayName,
        replyText,
        timestamp: new Date(),
        numUpVotes: 0,
        numDownVotes: 0,
      });
    },

    toggleReplies() {
      this.isRepliesVisible = !this.isRepliesVisible;
    },
  },

  filters: {
    fromNow(timestamp) {
      return moment(timestamp).fromNow(true);
    },
  },

  props: {
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
      .comment__body {}
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

    .replies {
      margin-right: 8px;
      margin-bottom: 24px;
      margin-left: 8px;
    }

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
