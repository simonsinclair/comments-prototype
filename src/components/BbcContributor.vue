<template>
  <div class="contributor">
    <img
      class="contributor__avatar"
      src="../assets/avatar-placeholder.svg"
      alt=""
      width="32" height="32" />
    <span class="contributor__name gel-pica-bold">{{ displayName }}</span>
  </div>
</template>

<script>
export default {
  props: {
    displayName: String,
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
