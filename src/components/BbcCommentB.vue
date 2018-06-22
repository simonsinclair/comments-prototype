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
    <transition name="drawer">
      <div class="replies" v-show="isRepliesVisible">
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
        <transition-group name="new-reply" tag="div">
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
          ref="submitComment"
          :placeholder-text="'Reply as ' + session.displayName"
          @comment-submitted="submitReply"
          accepts-media=""
          cta-text="Add reply">
        </bbc-submit-comment>
      </div>
    </transition>
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
      newReplies: [],
    };
  },

  methods: {
    doReply() {
      this.isRepliesVisible = true;
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

<style src="./BbcComment.scss" lang="scss" scoped=""></style>
