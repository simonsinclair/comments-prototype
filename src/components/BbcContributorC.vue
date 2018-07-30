<template>
  <div class="contributor">
    <div>
      <span class="contributor__name gel-pica-bold">{{ displayName }}</span>
      <span class="contributor__quotee" v-if="quote"><span>to</span> {{ quote.displayName }}</span>
    </div>
    <span class="contributor__timestamp gel-long-primer">{{ timestamp | fromNow }}</span>
  </div>
</template>

<script>
import moment from 'moment';

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
  props: {
    displayName: String,
    timestamp: Date,
    quote: Object,
  },

  filters: {
    fromNow(timestamp) {
      return moment(timestamp).fromNow();
    },
  },
};
</script>

<style lang="scss" scoped="">
  .contributor {}
    .contributor__name,
    .contributor__timestamp {}

    .contributor__name {}
    .contributor__quotee {
      color: #999;

      span {
        font-style: italic;
      }
    }
    .contributor__timestamp {
      color: #999;
    }
</style>
