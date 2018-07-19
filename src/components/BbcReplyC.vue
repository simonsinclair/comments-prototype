<template>
  <div class="reply-wrap" :id="uuid">
    <div class="reply">
      <div class="reply__header">
        <bbc-contributor :display-name="displayName"></bbc-contributor>
      </div>
      <div class="reply__body">
        <p class="gel-great-primer">{{ replyText }}</p>
      </div>
      <div class="reply__footer">
        <div class="gel-layout">
          <div class="gel-layout__item gel-1/2">
            <div class="gel-brevier">
              <bbc-reply-cta-c @reply="preReply()"></bbc-reply-cta-c>
              <span class="reply__timestamp">{{ timestamp | fromNow }}</span>
              <span class="reply__bullet">&bull;</span>
              <span class="reply__report"><a href="#">Report</a></span>
            </div>
          </div>
          <div class="gel-layout__item gel-1/2 reply__actions">
            <button class="gel-brevier">
              <img src="../assets/up-thumb.svg" alt="" /> {{ numUpVotes }}
            </button>
            <button class="gel-brevier">
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
        @reply-submitted="submitReply"
        @reply-cancelled="cancelReply"
        cta-text="Add reply">
      </bbc-submit-reply>
    </transition>

    <transition name="new-reply" tag="div">
      <div class="submit-reply-success" v-show="isSubmitReplySuccessVisible">
        <a :href="'#' + latestSubmitReplyUuid">View my reply</a>
      </div>
    </transition>
  </div>
</template>

<script>
import moment from 'moment';
import inView from 'in-view';

import BbcContributor from './BbcContributor';
import BbcSubmitReply from './BbcSubmitReply';
import BbcReplyCtaC from './BbcReplyCtaC';
import BbcReplyC from './BbcReplyC';

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
  data() {
    return {
      latestSubmitReplyUuid: '',
      isSubmitReplyVisible: false,
      isSubmitReplySuccessVisible: false,
    };
  },

  components: { BbcContributor, BbcSubmitReply, BbcReplyCtaC, BbcReplyC },

  filters: {
    fromNow(timestamp) {
      return moment(timestamp).fromNow(true);
    },
  },

  props: {
    session: Object,

    uuid: String,

    displayName: String,
    replyText: String,
    timestamp: Date,
    numUpVotes: Number,
    numDownVotes: Number,
  },

  methods: {
    preReply() {
      this.isSubmitReplySuccessVisible = false;
      this.isSubmitReplyVisible = true;

      this.$nextTick(() => {
        this.$refs.submitReply.focus();
      });
    },

    submitReply(replyText, uuid) {
      this.$emit('reply-to-reply-submitted', replyText, uuid);

      this.isSubmitReplyVisible = false;

      this.$nextTick(() => {
        this.afterReply(uuid);
      });
    },

    cancelReply() {
      this.isSubmitReplyVisible = false;
    },

    afterReply(replyUuid) {
      this.latestSubmitReplyUuid = replyUuid;

      if (!inView.is(document.getElementById(replyUuid))) {
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

