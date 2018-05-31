<template>
  <div class="contributor">
    <img class="contributor__avatar" :src="avatarSrc" alt="" width="32" height="32" />
    <span class="contributor__name gel-pica-bold">{{ displayName }}</span>
  </div>
</template>

<script>
export default {
  props: {
    displayName: String,
    avatarSrc: {
      type: String,
      default: 'https://placehold.it/64',
    },
  },
};
</script>

<style lang="scss" scoped="">
  .contributor {
    display: flex;
    align-items: center;
  }
  .contributor__avatar {
    border-radius: 50%;
    margin-right: 8px;
  }
  .contributor__name {}
</style>
