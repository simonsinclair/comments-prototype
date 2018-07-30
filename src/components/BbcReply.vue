<template>
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
</template>

<script>
import moment from 'moment';

import BbcContributor from './BbcContributor';

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
  components: { BbcContributor },
  filters: {
    fromNow(timestamp) {
      return moment(timestamp).fromNow(true);
    },
  },
  props: {
    displayName: String,
    replyText: String,
    timestamp: Date,
    numUpVotes: Number,
    numDownVotes: Number,
  },
};
</script>

<style lang="scss" scoped="">
  .reply {
    background-color: #FFF;
    margin-right: 12px;
    margin-bottom: 2px;
    margin-left: 12px;

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

