<template>
  <div class="noData">
    <div class="txt">{{ txt }}</div>
  </div>
</template>

<script>
export default {
  props: {
    txt: {
      type: String,
      default: ''
    }
  }
}
</script>

<style scoped lang="less">
.noData {
  text-align: center;
  padding-top: 100px;
  font-size: 16px;
  .txt {
    margin-top: 10px;
    color: #888;
  }
  > img {
    width: 100px;
  }
}
</style>
