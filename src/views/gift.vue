<template>
  <div class="gift">
    <vue-element-loading :active="loading" :is-full-screen="true" spinner="bar-fade-scale" color="#B29881" background-color="transparent"/>
    <div v-if="IsShow" id="promosContent" class="swiper-container">
      <div class="swiper-wrapper">
        <div class="promosContent swiper-slide">
          <div class="reg58">
            <ul>
              <li>
                <a :href="wapUrl" target="_blank">
                  <mt-button type="default" class="button">新用户点击注册(记得返回申请)</mt-button>
                </a>
              </li>
              <li>
                <span >游戏账号 :</span>
              <input v-model="account" maxlength="30" type="text" placeholder="新注册的游戏账号" class="account"></li>
              <li><span >联系电话 :</span>
              <input v-model="tel" maxlength="11" type="text" placeholder="收到短信的手机号" class="tel"></li>
              <li>
                <a>
                  <button class="mint-button button mint-button--default mint-button--normal" @click="PostFrom">
                    <label class="mint-button-text">提交申请(短信通知结果)</label>
                  </button>
                </a>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
    <div class="tableBox" style="margin-top: 20px" v-html="content"/>
    <div class="goIndex">
      <span class="tap-effect bgColor-effect" @click="toIndex">返回首页</span>
    </div>
  </div>
</template>

<script>
import '../assets/css/index.css'
import '../assets/css/proms.css'
import { getIndexData, PostFrom } from '../model/index.js'

export default {
  data() {
    return {
      content: '',
      activity: 1,
      wapUrl: '',
      tel: '',
      account: '',
      IsShow: true,
      loading: true
    }
  },
  created() {
    this.init()
  },
  methods: {
    init() {
      getIndexData().then(res => {
        this.loading = false
        for (var i = 0; i < res.data.data.List.length; i++) {
          switch (res.data.data.List[i].downkey) {
            case 'ToMobile':
              this.wapUrl = res.data.data.List[i].downurl
              break
            default:
          }
        }
        if (res.data.data.activity != null) {
          this.activity = res.data.data.activity.Atype
          if (res.data.data.activity.desc.indexOf('无需申请') >= 0) {
            this.IsShow = false
          }
          this.content = res.data.data.activity.content
        }
      })
    },
    toIndex() {
      this.$router.push({ name: 'index' })
    },
    PostFrom() {
      // 提交申请
      this.loading = true
      PostFrom({ tel: this.tel, account: this.account, aType: this.activity, AImg: '' }).then(res => {
        // eslint-disable-next-line eqeqeq
        this.loading = false
        this.tel = ''
        this.account = ''
        alert(res.data.msg)
      })
    }
  }
}
</script>

<style scoped lang="less">

</style>
