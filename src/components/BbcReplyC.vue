<template>
  <div class="reply-wrap" :id="id">
    <div class="reply">
      <div class="reply__header">
        <bbc-contributor-c :display-name="displayName" :timestamp="timestamp"></bbc-contributor-c>
      </div>
      <div class="reply__body">
        <!-- Temp. -->
        <div v-if="quote">
          <p>{{ quote.displayName }}</p>
          <p>{{ quote.replyText }}</p>
        </div>
        <p class="gel-great-primer">{{ replyText }}</p>
      </div>
      <div class="reply__footer">
        <div class="gel-layout">
          <div class="gel-layout__item gel-1/2">
            <div class="gel-brevier">
              <bbc-reply-cta-c @reply="preReply()"></bbc-reply-cta-c>
              <!-- <span class="reply__timestamp">{{ timestamp | fromNow }}</span> -->
              <!-- <span class="reply__bullet">&bull;</span> -->
              <!-- <span class="reply__report"><a href="#">Report</a></span> -->
            </div>
          </div>
          <div class="gel-layout__item gel-1/2 reply__actions">
            <button class="gel-brevier">
              <img src="../assets/up-thumb.svg" alt="" /> {{ numUpVotes }}
            </button>
            <button class="gel-brevier">
              <img src="../assets/down-thumb.svg" alt="" /> {{ numDownVotes }}
            </button>
            <button>
              <img src="../assets/gel-icon-more-vertical.svg" alt="" height="16" width="16" />
            </button>
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
  </div>
</template>

<script>
import _ from 'lodash';
// import moment from 'moment';
import inView from 'in-view';
import shortid from 'shortid';

import BbcContributorC from './BbcContributorC';
import BbcSubmitReply from './BbcSubmitReply';
import BbcReplyCtaC from './BbcReplyCtaC';
import BbcReplyC from './BbcReplyC';

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
  data() {
    return {
      latestReplyId: '',
      isSubmitReplyVisible: false,
      isSubmitReplySuccessVisible: false,
    };
  },

  components: { BbcContributorC, BbcSubmitReply, BbcReplyCtaC, BbcReplyC },

  // filters: {
  //   fromNow(timestamp) {
  //     return moment(timestamp).fromNow(true);
  //   },
  // },

  props: {
    session: Object,

    id: String,
    displayName: String,
    replyToId: {
      type: String,
      required: true,
    },
    replyText: String,
    timestamp: Date,
    numUpVotes: Number,
    numDownVotes: Number,

    // For searching replies to.
    replyStack: {
      type: Array,
      required: true,
    },

    newReplyStack: {
      type: Array,
      required: true,
    },
  },

  computed: {
    quote() {
      const stack = _.concat(this.replyStack, this.newReplyStack);
      return _.find(stack, ['id', this.replyToId]);
    },
  },

  methods: {
    preReply() {
      this.isSubmitReplySuccessVisible = false;
      this.isSubmitReplyVisible = true;

      this.$nextTick(() => {
        this.$refs.submitReply.focus();
      });
    },

    submitReply(replyText, replyingToId) {
      const newReplyId = shortid.generate();

      this.$emit('reply-to-reply-submitted', newReplyId, replyText, replyingToId);

      this.isSubmitReplyVisible = false;

      this.$nextTick(() => {
        this.afterReply(newReplyId);
      });
    },

    cancelReply() {
      this.isSubmitReplyVisible = false;
    },

    afterReply(newReplyId) {
      this.latestReplyId = newReplyId;

      if (!inView.is(document.getElementById(newReplyId))) {
        this.isSubmitReplySuccessVisible = true;
      }
    },

    getReplies(rawReplies) {
      return rawReplies;
    },
  },
};
</script>

<style lang="scss" scoped="">
  .reply-wrap {
    padding-left: 24px;
  }

  .reply {
    background-color: #FFF;
    margin-bottom: 2px;

    &:last-child {
      margin-bottom: 12px;
    }
  }
  .reply__header,
  .reply__body {
    padding-right: 12px;
    padding-left: 12px;
  }
  .reply__header {
    padding-top: 12px;
  }
  .reply__body {

    > p {
      white-space: pre-line;
    }
  }
  .reply__footer {

    button {
      padding: 12px;
      padding-top: 0;
    }
  }

    .reply__actions {
      text-align: right;
      padding-right: 8px;

      button {
        padding: 0 8px 12px;
      }
    }


    .reply__timestamp,
    .reply__bullet,
    .reply__report {
      display: inline-block;
    }
    .reply__timestamp {
      margin-left: 12px;
    }
    .reply__bullet {
      margin-left: 4px;
    }
    .reply__report {
      margin-left: 4px;
    }
</style>

