<template>
  <div class="comment-wrap">

    <!-- Comment -->
    <div class="comment">
      <div class="comment__header">
        <img src="http://placehold.it/32" alt="" />
        <span class="comment__display-name">{{ displayName }}</span>
        <!--
          Todo:
            - Render special flag, e.g. Editors pick
        -->
      </div>
      <div class="comment__body">
        <p>{{ commentText }}</p>
        <span class="comment__timestamp">{{ timestamp }}</span>
      </div>
      <div class="comment__footer">
        <button>{{ replies.length }} Replies</button>

        <button>Report</button>
        <button>Up {{ numUpVotes }}</button>
        <button>Down {{ numDownVotes }}</button>
      </div>
    </div>

    <!-- Comment Replies -->
    <div class="replies">
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

export default {
  components: { BbcReply, BbcAddReply },
  props: {
    displayName: String,
    commentText: String,
    timestamp: Number,
    numUpVotes: Number,
    numDownVotes: Number,
    replies: Array,
  },
};
</script>

<style lang="scss" scoped="">
  .comment-wrap {
    padding: 8px;
    margin-bottom: 16px;
  }
    .comment {
      background-color: #FFF;
    }
      .comment__header {}
        .comment__display-name {}
      .comment__body {}
      .comment__footer {}

    .replies {}
</style>
