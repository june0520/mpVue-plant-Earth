<template>
  <div class="container">
    <button class="btn" open-type="getUserInfo" @getuserinfo="getUserInfo" v-if="isAvatarUrl">点击获取用户信息</button>
    <img :src="userInfo.avatarUrl" alt="" v-else>
    <p class="hello">Hello {{ userInfo.nickName }}</p>
    <div class="jump">
      <p @click="jump">开启小程序之旅</p>
    </div> 
  </div>
</template>

<script>
import card from '@/components/card'

export default {
  data () {
    return {
      userInfo:{},
      isAvatarUrl: true
    }
    
  },

  components: {
    card
  },

  methods: {
    handleUserInfo(){
      var that = this
      wx.getUserInfo({
        success(res) {
            that.userInfo = res.userInfo
          } 
        })
    },
    getUserInfo(data){
      if(data.mp.detail.rawData){
        this.isAvatarUrl = false
        this.handleUserInfo()
      }
    },
    jump(){
      wx.navigateTo({
        url: "/pages/detail/main"
      })
    }

  },

  created () {
    this.handleUserInfo()
  }
}
</script>

<style scoped>
page {
  background-color: #8ed130;
}
.index-wrap {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.btn {
  display:flex;
  justify-content: center;
  align-items: center;
  margin-top: 100rpx;
  width: 200rpx;
  height: 200rpx;
  border-radius: 50%;
  border: 2px solid #333;
  font-size: 24rpx;
}
img{
  margin-top: 100rpx;
  width: 200rpx;
  height: 200rpx;
  border-radius: 50%;
}
.hello {
  margin-top:100rpx;
  font-size: 40rpx;
  font-weight: 700;
}
.jump {
  display:flex;
  justify-content: center;
  align-items: center;
  margin-top: 200rpx;
  width: 200rpx;
  height: 80rpx;
  border: 2px solid #333;
  border-radius: 20rpx;
}
.jump p {
  font-size:28rpx;
}

</style>
