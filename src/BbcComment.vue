<template>
  <div class="comment-wrap">

    <!-- Comment -->
    <div class="comment">
      <div class="comment__header">
        <bbc-contributor :display-name="displayName"></bbc-contributor>
        <!-- <img src="https://placehold.it/32" alt="" />
        <span class="comment__display-name gel-pica-bold">{{ displayName }}</span> -->
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
        <button class="gel-pica-bold"
          @click="toggleReplies()"
          v-show="!isRepliesVisible">
          {{ replies.length > 0 ? `${replies.length} Replies` : 'Reply' }}
        </button>
        <button class="gel-pica-bold"
          @click="toggleReplies()"
          v-show="isRepliesVisible">
          Close
        </button>

        <button class="gel-pica-bold">Report</button>
        <button class="gel-pica">Up {{ numUpVotes }}</button>
        <button class="gel-pica">Down {{ numDownVotes }}</button>
      </div>
    </div>

    <!-- Comment Replies -->
    <div class="replies" v-show="isRepliesVisible">
      <bbc-reply
        v-for="reply in replies"
        :key="reply.id"

        :display-name="reply.displayName"
        :reply-text="reply.replyText"
        :num-up-votes="reply.numUpVotes"
        :num-down-votes="reply.numDownVotes"
      ></bbc-reply>
      <bbc-add-reply :display-name="displayName" :replies="replies"></bbc-add-reply>
    </div>
  </div>
</template>

<script>
import moment from 'moment';

import BbcReply from './BbcReply';
import BbcAddReply from './BbcAddReply';
import BbcContributor from './BbcContributor';

export default {
  components: { BbcReply, BbcAddReply, BbcContributor },
  data() {
    return {
      isRepliesVisible: false,
    };
  },
  methods: {
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
</style>
