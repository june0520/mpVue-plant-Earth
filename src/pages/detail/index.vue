<template>
  <div class="container" :style="{'transform': 'translate(0,'+topNum*100+'%)'}" @touchstart="startHandle($event)" @touchmove="moveHandle($event)" @touchend="endHandle" >
      <div class="page page1">
        <img src="/static/index.jpg" alt="" class='bg_img'>
        <h2 class="title"> Plante  Earth</h2>
        <img src="/static/page1Btn.png" alt="" class="btn" @click.stop="toPage2">
        <button class="btn2" open-type="getUserInfo" @getuserinfo="getUserInfo" v-if="isAvatarUrl"></button>


      </div>
      <div class="page page2">
            <img src="/static/page.jpg" alt="" class='bg_img'>
            <div class="pageTxtWrap">
                <p class="p1 pageTxt" :class="{'pageInto': inPage2}">2018年，全球共发生六级以上地震<span class="num">119</span>次,</p>
                <p class="p2 pageTxt" :class="{'pageInto': inPage2}">其中6.0级<span class="num">101</span>次,</p>
                <p class="p3 pageTxt" :class="{'pageInto': inPage2}">7.0级到7.9级<span class="num">16</span>次,</p>
                <p class="p4 pageTxt" :class="{'pageInto': inPage2}">8.0级以上<span class="num">2</span>次,</p>
                <p class="p5 pageTxt" :class="{'pageInto': inPage2}">最大地震是8月19日在<span class="num">斐济群岛地区</span>发生</p>
                <p class="p6 pageTxt" :class="{'pageInto': inPage2}" >的8.1级地震。</p>
            </div>
            <img src="/static/jiantou.png" alt="" mode="aspectFit" class="jiantou">
      </div>
      <div class="page page3">
          <img src="/static/page2.jpg" alt="" class="bg_img">
        <div class="pageTxtWrap">
            <p class="p0 pageTxt" :class="{'pageInto': inPage3}">2018年，我国共发生三级以上地震<span class="num">542</span>次,<br/><span class="smallW">(台湾地区4.0级以下地震未统计在内)</span></p>
            <p class="p2 pageTxt" :class="{'pageInto': inPage3}">其中3.0级到3.9级<span class="num">363</span>次,</p>
            <p class="p3 pageTxt" :class="{'pageInto': inPage3}">4.0级到4.9级<span class="num">148</span>次, 5.0级到5.9级<span class="num">26</span>次,</p>
            <p class="p4 pageTxt" :class="{'pageInto': inPage3}">6.0级到6.9级<span class="num">5</span>次, 7.0级以上<span class="num">0</span>次,</p>
            <p class="p5 pageTxt" :class="{'pageInto': inPage3}">最大地震是2月6日在台湾花莲县附近海域发</p>
            <p class="p6 pageTxt" :class="{'pageInto': inPage3}" >生的<span class="num">6.5</span>级地震。</p>
        </div>
        <img src="/static/jiantou.png"   mode="aspectFit" alt="" class="jiantou">
    </div>
    <div class="page page4" >
      <img src="/static/page4.jpg" mode="aspectFill" alt="" class="bg_img">
      <img src="/static/page4Btn.png" mode="aspectFit" alt="" class="page4btn" @click="longTap">
      <p class="page4txt" >想知道<br/>你身边的地震活动分布吗</p>
      <div class="page4box">
          <img src="/static/zhiwei.png"  mode="aspectFit" alt="" class="zhiwei">
          <div class="point" @longpress="longTap">
            <div class="page4yuan"></div>
          </div>
      </div>
    </div>
    <div class="page page5">
      <canvas class="canvas" canvas-id="myCanvas" :hidden="canvasHidden"></canvas>
      <img src="/static/last.jpg" mode="aspectFill" alt="" class="bg_img">
      <div class="box5Wrap">
        <img src="/static/page5txtBox2.png" mode="widthFix" alt="" class="page5txtBox">
        <img src="/static/page5Btn.png" mode="aspectFit" alt="" @click="saveImg" class="page5btn">
        <div class="touxiangWrap">
          <open-data type="userAvatarUrl" class="touxiang"></open-data>
        </div>
        <div class="page5txt">
            <p>{{p1}}</p>
            <p>{{p2}}</p>
            <p>{{p3}}</p>
        </div>
        <!-- <p class="page5tip" :class="{'showTip': true}">地震就在我们的身边， 要学会与地震和谐相处</p> -->
      </div>
    </div>
  </div>
</template>
<script>     
export default {
    data(){
        return {
            isAvatarUrl: true,
            avatarUrl: '',
            creenHeight: 0,
            topNum: 0,
            inPage2: false,
            inPage3: false,
            startY: 0, //滑动开始y轴位置
            lastY: 0,   //滑动结束y轴位置
            p1: '',
            p2: '',
            p3: '',
            p1txt: '我身边500公里内近10年来曾经发生过三级以上的地震98次，其中：',
            p2txt: '最近的地震是2015年07月05日在河北省张家口市涿鹿县发生的3.2级地震，震中距离我约69公里。',
            p3txt: '最大的地震时2012年05月28日在河北省唐山市市辖区，滦县交界发生的4.8级地震，震中距我约202公里。',
            i: 0,
            len: 0,  //文本长度
            j: '',
            tip: false, // 最后一页蓝色小字是否显示
            canvasHidden: true,
            canvasDone: false,
        }
    },
    methods: {
    handleUserInfo(){
      var that = this
      wx.getUserInfo({
        success(res) {
            that.isAvatarUrl = false
            that.avatarUrl = res.userInfo.avatarUrl
            console.log(that.avatarUrl)
          } 
        })
    },
    getUserInfo(data){
      if(data.mp.detail.rawData){
        this.isAvatarUrl = false
        this.handleUserInfo()
      }
    },
    toPage2(){
      this.handleUserInfo()

      this.topNum = -1
      if(this.inPage2){
        return
      }
      setTimeout(()=> {
        this.inPage2 = true
        }, 500)
    },
    startHandle(e){
        this.startY = e.touches[0].pageY
    },
    moveHandle(e){
        this.lastY  = e.touches[0].pageY
    },
    endHandle(){
        if(this.lastY === 0){
            return 
        }
        let ty = this.lastY - this.startY
        if(ty < -30){
            if(this.topNum !== -3 && this.topNum !== -4 && this.topNum !== 0){
                this.topNum--
            }
        }
        if(ty > 30){
            if(this.topNum !== 0){
                this.topNum++
            }
        }
        this.lastY = 0
        if(this.topNum === -2){
            setTimeout(() => {
                this.inPage3 = true
            },500)
        }
    },
    longTap (){
        this.topNum = -4
        setTimeout(()=>{//加载页面文字
            this.showTxt1()
        },1000)
    },
    showTxt1(){
      if(this.p1!==''){
        return
      }
      this.canvasImg()

      let timer = setInterval(() => {
          if(this.i < this.p1txt.length){
              this.p1+=this.p1txt[this.i]
              this.i++
          } 
          if (this.i>=this.p1txt.length){
              clearInterval(timer)
              this.showTxt2()
          }
      },100)  
    },
    showTxt2(){
        this.i = 0
        let timer = setInterval(() => {
            if(this.i < this.p2txt.length){
                this.p2+=this.p2txt[this.i]
                this.i++
            } 
            if (this.i>=this.p2txt.length){
                clearInterval(timer)
                console.log("清chu定时器")
                this.showTxt3()
        }
    },100)  
  },
    showTxt3(){
       this.i = 0
        let timer = setInterval(() => {
            if(this.i < this.p3txt.length){
                this.p3+=this.p3txt[this.i]
                this.i++
            } 
            if (this.i>=this.p3txt.length){
                clearInterval(timer)
                console.log("清chu定时器")
        }
    },100)  
  },
    saveImg (){
      this.canvasImg()

      if(!this.canvasHidden){
        console.log('图片正在生成中')
        return 
      }
      this.canvasHidden = false
      var that = this
      wx.showLoading({
        title: '正在生成图片',
        mask: true
    })

      setTimeout(function(){
        wx.canvasToTempFilePath({
          canvasId:'myCanvas',
          fileType: "png",
          width: 750,
          height: 1334,
          destWidth: 750,
          destHeight: 1334,
          quality: 1,
          success(res){
            wx.authorize({
              scope: "scope.writePhotosAlbum",
              success(){
                wx.saveImageToPhotosAlbum({
                  filePath: res.tempFilePath,
                  success(){
                    that.canvasHidden = true
                    wx.showModal({
                      title: '存图成功',
                      content: '图片保存到相册了，快去分享朋友圈吧',
                      showCancel: false,
                      confirmText: '好的',
                      confirmColor: "#72B9C3",
                      success: function(res){
                        if(res.confirm){
                          console.log('用户点击了同意')
                        }
                      }
                    })
                  }
                })
              },
              fail(){
                wx.showModal({
                  title: '获取权限失败',
                  content: '您点击了拒绝权限，将无法保存图片到相册，点击确定重新获取授权',
                  success: function(res){
                    if(res.confirm){
                      wx.openSetting({
                        success: function(res){
                          that.canvasHidden = true
                        }
                      })
                    }else {
                      console.log('保存相册权限获取失败')
                    }
                  }
                })
              },
              complete(){
                wx.hideLoading()
              }
            })
          }
        })
      },500)
    },
    canvasImg(){
      var that = this
      if(this.canvasDone){
        return
      }

      let scalc = 2
      const ctx = wx.createCanvasContext('myCanvas')
      let promise1 = new Promise(function(resolve,reject){
        wx.getImageInfo({
          src: that.avatarUrl,
          success: function(res){
            console.log("开始绘图")
            console.log('头像本地路径', res.path)
            resolve(res)
          },
          fail(res){
            console.log('下载头像失败了',res)
          }
        })
      }).then((res) => {
        console.log("promise   ",res)
        ctx.drawImage('/static/last.jpg',0,0,375*scalc,667*scalc)
        console.log('1')
        ctx.save()
        ctx.beginPath()
        ctx.arc(187.75*scalc, 124.25*scalc, 40*scalc, 0, 2 * Math.PI,false) 
        console.log('2')
        ctx.clip()
        ctx.drawImage(res.path,147.75*scalc, 84.25*scalc, 80*scalc, 80*scalc)
        console.log('3')
        ctx.restore()//恢复之前保存的绘图上下文
        ctx.font = "bold 28px PingFangSC"
        ctx.setFillStyle('#fff')
        this.fillTextWrap(ctx, '       '+this.p1txt, 48*scalc,230*scalc, 280*scalc, 24*scalc)
        this.fillTextWrap(ctx, '       '+this.p2txt, 48*scalc,288*scalc, 280*scalc, 24*scalc)
        this.fillTextWrap(ctx, '       '+this.p3txt, 48*scalc,370*scalc, 280*scalc, 24*scalc)
        ctx.draw()//绘制到canvas
        that.canvasHidden = true
        that.canvasDone = true
        console.log('canvas绘制完毕')
        }
      )
    },

    fillTextWrap(ctx, text, x, y, maxWidth, lineHeight) {
      // 设定默认最大宽度
      const systemInfo = wx.getSystemInfoSync();
      if (typeof text !== 'string' || typeof x !== 'number' || typeof y !== 'number') {
        return;
      }
      // 字符串分割为数组
      const arrText = text.split('');
      // 当前字符串及宽度
      let currentText = '';
      let currentWidth;
      let long = 0;  // 根据字符的个数来换行
      for (let letter of arrText) {
        currentText += letter;
        if (letter == '') {
          long+=0.25;
        } else if ( letter == '.'){
          long+=1.5
        } else if ( isNaN(letter)) {
          long+=1
        } else if ( !isNaN(letter)){
          long+=0.5
        } 
        if( long >= 20.5 ){
          ctx.fillText(currentText, x, y);
          currentText = '';
          long = 0;
          y += lineHeight;
        }
      }
      if (currentText) {
        ctx.fillText(currentText, x, y);
      }
    },
  },
  mounted(){
    let that = this
    wx.getUserInfo({
      success(res){
        that.avatarUrl = res.userInfo.avatarUrl
      },fail(err){
        console.log(err)
      }
    })
  }
}
</script>
<style scoped> 
.container {
  position: fixed;
  width: 100%;
  height: 100%;
  left: 0;
  transition: transform 1s linear;  
}
.page {
    position: fixed;
    width: 100%;
    height: 100%;
    left: 0;
}
.title {
  position: absolute;
  left: 0;
  top: 260rpx;
  width: 100%;
  text-align: center;
  color: #fff;
  font-size: 90rpx;
}
.page1 {
    top: 0;
}
.page2 {
    top: 100%;
}
.page3 {
    top: 200%;
} 
.page4 {
    top: 300%;
} 
.page5 {
    top: 400%;
} 

.bg_img {
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  right: 0;
}
/* .title {
    position: absolute;
    top: 80rpx;
    left: 50%;
    margin-left: -200rpx;
    width: 600rpx;
    height: 400rpx;
} */
.btn {
    position: absolute;
    bottom: 30rpx;
    left: 50%;
    margin-left: -250rpx;
    width: 500rpx;
    height: 200rpx;
}
.btn2 {
    opacity: 0;
    position: absolute;
    bottom: 30rpx;
    left: 50%;
    margin-left: -250rpx;
    width: 500rpx;
    height: 200rpx;
}
.pageTxtWrap {
  width: 100%;
  height: 100%; 
  position: absolute;
  top: 150rpx;
  left: 0;
  font-family: "PingFangSC";
  text-align: left;
  font-size: 32rpx;
  color: #fff;
  text-shadow: 0px 0px 30px rgba(55, 72, 176, 0.45);
  letter-spacing: 3.5rpx;
  font-weight: 600;
}
.pageTxt {
  position: absolute;
  left: 50rpx;
  opacity: 0;
}
.p0 {
  top: 360rpx;
  transition: all 1.5s ease 0s;
}
.p1 {
  top: 400rpx;
  transition: all 1.5s ease 0s;
}
.p2 {
  top: 470rpx;
  transition: all 1.5s ease 0.5s;
}
.p3 {
  top: 540rpx;
  transition: all 1.5s ease 1s;
}
.p4 {
  top: 610rpx;
  transition: all  1.5s ease 1.5s;
}
.p5 {
  top: 680rpx;
  transition: all 1.5s ease 2s;
}
.p6 {
  top: 740rpx;
  transition: all 1.5s ease 2.5s;
}
.pageInto {
  transform: translate(0, -340rpx);
  opacity: 1;
}
.num {
  font-size: 36rpx;
  color: red;
}
.jiantou {
  position: absolute;
  width: 30rpx;
  height: 62rpx;
  left: calc( 50% - 15rpx);
  bottom: 22rpx;
  animation: up 2s linear infinite;
}
@keyframes up {
  0% {
    opacity: 1;
  }
  50% {
    opacity: .3;
  }
  100% {
    opacity: 1;
  }
}
.smallW {
  font-size: 26rpx;
}
.page4btn {
  position: absolute;
  width: 370rpx;
  height: 200rpx;
  bottom: 30rpx;
  left: calc( 50% - 185rpx );
}  
.page4box {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
  width: 600rpx;
  height: 700rpx;
  opacity: .85;
}
.zhiwei {
  position: absolute;
  top: 100rpx;
  left: 60rpx;
  width: 480rpx;
  height: 480rpx;
}
.page4txt {
  font-family: "PingFangSC";
  width: 100%;
  position: absolute;
  top: 100rpx;
  text-align: center;
  font-size: 40rpx;
  color: #fff;
  text-shadow: 0px 0px 30px rgba(55, 72, 176, 0.45);
  letter-spacing: 4rpx;
  font-weight: 600;
  line-height: 60rpx;
}
.point {
  width:410rpx;
  height:410rpx;
  margin:139rpx auto;
}
.page4yuan {
  width: 205rpx;
  height: 205rpx;
  border-radius: 205rpx 0 0;
  background: #fff;
  opacity: 0.4;
  animation: rotate 3s  linear infinite ;
  transform-origin: bottom right; 

}
@keyframes rotate {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
.box5Wrap {
  width:100%;
  height: auto;
  margin: 160rpx auto 0;
  position: relative;  
}
.page5txtBox {

  width:100%;
  height: auto;
}
.page5btn {
  width: 500rpx;
  height: 200rpx;
  margin-top: -140rpx;
  margin-left:calc( 50% - 250rpx);

}
.touxiangWrap {
  width: 164rpx;
  height: 164rpx;
  border-radius: 50%;
  position: absolute;
  top: 19rpx;
  left: calc( 50% - 82rpx);
  overflow: hidden;
}
.touxiang {
  width: 164rpx;
  height: 164rpx;
}
.page5txt {
  position: absolute;
  top: 200rpx;
  width: 100%;
  padding-top:50rpx;
}
.page5txt p {
  margin: auto;
  width: 590rpx;
  font-size: 30rpx;
  font-family: "PingFangSC";
  color: rgb(255, 255, 255);
  line-height: 1.667;
  text-indent: 2em;
  text-shadow: 0px 0px 30px rgba(55, 72, 176, 0.45);
  letter-spacing: 2rpx;
  margin-top: 20rpx;
  font-weight: 600;
  transition: all .5s;
}
.page5tip {
  width: 100%;
  text-align: center;
  position: absolute;
  bottom: 200rpx;
  left: 0;
  opacity: 0;
  font-size: 22rpx;
  font-family: "PingFangSC";
  color: rgb(109, 211, 248);
  text-shadow: 0px 0px 30px rgba(55, 72, 176, 0.45);
  transition: all 2s;
}
.showTip {
  opacity: 1;
}
.canvas {
  width:750px;
  height:1334px;
  position: fixed; 
  left: 2000px;
  top:2000px
}
</style>   
