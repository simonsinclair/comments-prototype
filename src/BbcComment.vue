<template>
  <div class="comment-wrap">

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
        <p>{{ commentText }}</p>
        <span class="comment__timestamp gel-brevier">{{ timestamp | fromNow }}</span>
      </div>
      <div class="comment__footer">
        <!-- Better way to do this? -->
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

        <button class="gel-pica-bold">Report</button>
        <button class="gel-pica">
          <img src="./assets/up-thumb.svg" alt="" /> {{ numUpVotes }}
        </button>
        <button class="gel-pica">
          <img src="./assets/down-thumb.svg" alt="" /> {{ numDownVotes }}
        </button>
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
          :num-up-votes="reply.numUpVotes"
          :num-down-votes="reply.numDownVotes"
        ></bbc-reply>
      </transition-group>
      <bbc-submit-comment
        :display-name="displayName"
        @comment-submitted="submitReply"
        accepts-media=""
        placeholder-text="Add your reply"
        cta-text="Add reply">
      </bbc-submit-comment>
    </div>
  </div>
</template>

<script>
import moment from 'moment';

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
      return moment(timestamp).fromNow();
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
    margin-bottom: 16px;
  }
    .comment {
      background-color: #FFF;
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
      .comment__footer {

        > button {
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
