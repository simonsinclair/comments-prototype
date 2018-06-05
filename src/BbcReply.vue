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
        <div class="gel-layout__item gel-1/2@m">
          <div class="gel-brevier">
            <span class="comment__timestamp">{{ timestamp | fromNow }}</span>
            <span class="comment__bullet">&bull;</span>
            <span class="comment__report"><a href="#">Report</a></span>
          </div>
        </div>
        <div class="gel-layout__item gel-1/2@m">
          <button class="gel-brevier">
            <img src="./assets/up-thumb.svg" alt="" /> {{ numUpVotes }}
          </button>
          <button class="gel-brevier">
            <img src="./assets/down-thumb.svg" alt="" /> {{ numDownVotes }}
          </button>
          <button><img src="./assets/share.svg" alt="" /></button>
        </div>
      </div>
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

import BbcContributor from './BbcContributor';

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
    border-radius: 4px;
    margin-bottom: 20px;
    margin-left: 20px;
  }
  .reply__header,
  .reply__body {
    padding-right: 12px;
    padding-left: 12px;
  }
  .reply__header {
    padding-top: 12px;
  }
  .reply__body {}
  .reply__footer {

    button {
      padding: 12px;

      // Medium only -> 600px
      @media (min-width: 600px) {
        padding-top: 0;
      }
    }
  }
    .comment__timestamp,
    .comment__bullet,
    .comment__report {
      display: inline-block;
    }
    .comment__timestamp {
      margin-left: 12px;
    }
    .comment__bullet {
      margin-left: 4px;
    }
    .comment__report {
      margin-left: 4px;
    }
</style>

