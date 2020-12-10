<template>
  <div class="tabBar clearfix">
    <div v-for="(item,index) in list" :class="{'active':activeIndex==index}" :key="index" class="item" @click="toUrl(item)">{{ item.name }}</div>
  </div>
</template>

<script>
export default {
  props: {
    activeIndex: {
      type: Number,
      default: 0
    },
    list: {
      type: Array,
      default: () => []
    }
  },
  methods: {
    toUrl(item) {
      this.$router.replace({
        path: item.url
      })
    }
  }
}
</script>

<style scoped lang="less">
.tabBar {
  // position: fixed;
  // top: 0px;
  width: 100%;
  height: 45px;
  line-height: 45px;
  padding: 0 15px;
  background-color: #fff;
  z-index: 99;
  .item {
    width: 50%;
    text-align: center;
    float: left;
    font-size: 16px;
  }
  .active {
    height: 44px;
    color: #597ef7;
    border-bottom: 3px solid #597ef7;
  }
}
</style>
