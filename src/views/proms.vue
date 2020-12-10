<template>
  <div class="proms router-view">
    <div id="navBar" style="background-color: rgba(0,0,0,0)">
      <div ref="tabBar" class="tab-wrp" @mousedown="tabMouseDown($event)" @mousemove="tabMouseMove($event)" @mouseup="tabMouseUp($event)">
        <div v-for="(tab,index) in list" :style="`${index!=0?'margin-left: 20px':''}`" :key="tab.title" class="navItem" @click="tabClick(index)">
          <span :class="`${nowIndex==index?'slide-nav-active':''}`" :style="`${nowIndex==index?'background: #d8cbbe !important;color:#000':'color:#fff'}`" >{{ tab.title }}</span>
        </div>
        <div class="navItem" style="margin-left: 20px" @click="toIndex" >
          <span style="color: #fff">返回首页</span>
        </div>
      </div>
    </div>
    <vue-element-loading :active="loading" :is-full-screen="true" spinner="bar-fade-scale" color="#B29881" background-color="transparent"/>
    <div id="promosContent" class="swiper-container">
      <div class="swiper-wrapper">
        <div v-for="(tb,index) in list" :key="index" class="promosContent swiper-slide">
          <div class="first">
            <div class="gift">
              <div class="tableBox" v-html="tb.content"/>
              <div class="goIndex">
                <span class="tap-effect bgColor-effect" @click="toIndex">返回首页</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import '../assets/css/proms.css'
import Swiper from 'swiper'
import 'swiper/css/swiper.min.css'
import { getPromsData } from '../model/index'

export default {
  components: { Swiper },
  data() {
    return {
      nowIndex: 0,
      mySwiper: null,
      tabMouseState: 'up',
      startX: 0,
      startY: 0,
      moveDirection: 0,
      list: [],
      loading: true
    }
  },
  mounted() {
    var that = this
    window.onresize = () => {
      that.initSwiper()
    }
  },
  created() {
    this.initData()
  },
  methods: {
    toIndex() {
      console.log('toIndex')
      this.$router.push({ name: 'index' })
    },
    initData() {
      getPromsData('无需申请').then(res => {
        this.loading = false
        this.list = res.data.data.List
        this.initSwiper()
      })
    },
    initSwiper() {
      var that = this
      console.log('initSwiper')
      that.mySwiper = new Swiper('.swiper-container', {
        direction: 'horizontal',
        width: window.innerWidth < 1199 ? window.innerWidth : document.getElementById('promosContent').offsetWidth
      })
      that.mySwiper.init()
      console.log(that.mySwiper)
    },
    tabClick(index) {
      this.mySwiper.update()
      this.nowIndex = index
      this.mySwiper.slideTo(index, 300, function(e) { console.log(e) })
    },
    tabMouseDown(e) {
      this.tabMouseState = 'down'
      this.startX = e.clientX
      this.startY = e.clientY
    },
    tabMouseMove(e) {
      if (this.tabMouseState === 'down') {
        this.$refs.tabBar.scrollBy(this.startX - e.clientX, 0)
      }
    },
    tabMouseUp(e) {
      this.tabMouseState = 'up'
    }
  }
}
</script>

<style scoped lang="less">

</style>
