<!--
 * @Descripttion: “提示”添加到我到小程序
 * @version: 
 * @Author: 
 * @Date: 2019-12-09 10:39:33
 * @LastEditors: Henry
 * @LastEditTime: 2020-04-07 23:57:39
 -->
<template>
  <div  v-if="show">
    <div class="tip" @click="closed" :style="styleAll" :class="{'parameter':top}">
      <div class="tip-text">
        <div class="left">
          点击
          <img
            class="tip-icon"
            src="https://img1.mklimg.com/g4/M00/69/BA/rBBreV3tqw6ALz4uAAABxwQBd8U910.png!"
            alt
          /> {{text}}
        </div>
        <img @click="closed" class="close" src="https://img1.mklimg.com/g4/M00/71/02/rBBreV4DEwGAdTVFAAAEMz0TAAo198.png!" alt="">
      </div>
    </div>
    <div class="arrow" :style="stylePart"></div>
  </div>
</template>
<script>
  const menu = wx.getMenuButtonBoundingClientRect() // 获取胶囊定位
  export default {
    props: ['top'], // top 作为系统nav标示
    data () {
      return {
        show: false,
        text: '添加到「我的小程序」，找我更方便',
        systemInfoWidth: '',
        styleAll: '',
        stylePart: '',
        arrowTop: 11, // 箭头定位高度
        tipsTop: 5 // 提示框定位高度
      }
    },
    onReady () {
      this.getTip()
    },
    methods: {
      getTip () {
        var that = this
        wx.getSystemInfo({ // 系统信息
          success (res) {
            that.systemInfoWidth = res.screenWidth
            /**
             * 获取导航高度
             * 'iPhone': 64,
             * 'iPhone X': 88,
             * 'android': 68 */
            if (res.model.indexOf('iPhone X') !== -1 || res.model.indexOf('iPhone12') !== -1 || res.model.indexOf('unknown') !== -1) {
              that.tipsTop = 7
              that.arrowTop = 15
            } else if (res.model.indexOf('iPhone') !== -1) {} else {}
          }
        })
        // 自定义nav
        this.stylePart = `right:${this.systemInfoWidth - menu.right + menu.width * 0.72 - 5}px;top:${menu.bottom + 11}px`
        this.styleAll = `top:${menu.bottom + 17.5}px;`
        // 小程序系统nav,并且判断iphoneX 系列
        if (this.top) {
          this.styleAll = `top:${menu.bottom / this.tipsTop}px;`
          this.stylePart = `right:${this.systemInfoWidth - menu.right + menu.width * 0.72 - 5}px;top:${menu.bottom / this.arrowTop}px`
        }
        let flag = true
        if (flag) {
          this.show = true
        } else {
          this.show = false
        }
      },
      // 关闭提示
      closed () {
        this.$store.commit('tokenStore/setisTipFlag', false)
        this.getTip()
      }
    }
  }
</script>
<style lang="less" scoped>
  .arrow {
    position: fixed;
    top: -12rpx;
    right: 97rpx;
    width: 0;
    height: 0;
    border-width: 8px;
    border-style: solid;
    z-index: 1000;
    border-color: rgb(0,0,0) rgb(0,0,0) transparent transparent;
    transform: rotate(-45deg);
  }
  .tip {
    position: fixed;
    top: 120rpx;
    right: 24rpx;
    left: 24rpx;
    line-height: 95rpx;
    z-index: 1000;
    font-size: 0;

    background:rgb(0,0,0);
    border-radius: 80rpx;
    height: 95rpx;
    text-align: center;

    .tipBg {
      display: block;
      height: 121rpx;
      width:100%
    }
    &.parameter {
      top: 20rpx;
    }

    .tip-text {
      padding:0 32rpx;
      display: flex;
      align-items: center;
      justify-content: space-between;
      font-size: 28rpx;
      color: white;
      .left {
        display: flex;
        align-items: center;
        text-align: center;
      }
      .tip-icon {
        width: 70rpx;
        height: 40rpx;
        margin: 0 6rpx;
      }
      .close {
        width: 35rpx;
        height: 35rpx;
        display: inline-block;
      }
    }

  }

</style>
