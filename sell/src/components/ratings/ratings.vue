<template>
  <div class="ratings">
    <div class="ratings-container" @click="alertName">
      <span>评论区</span>
      <span>待完善</span>
    </div>

    <div class="alert-name" v-show="showName">
      <div class="alert-title">
        <i class="icon-close" @click="hiddenName"></i>
      </div>
      <img src="tabweng.jpg" width="80px" height="80px">
      <span>coded by TabWeng</span>
    </div>

  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    data(){
      return {
        showName:true
      }
    },
    methods: {
      alertName(){
        this.showName = true
      },
      hiddenName(){
        this.showName = false
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">

.ratings
  position: relative
  width: 100%
  padding-bottom: 100%
  .ratings-container
    position: absolute
    top: 50%
    left: 50%
    transform: translate(-50%, -50%)
    font-size: 0
    span
      display: block
      margin-top: 10px
      font-size: 28px
      color: #999
  .alert-name
    position: absolute
    top: 50%
    left: 50%
    transform: translate(-50%, -50%)
    width: 50%
    height: 150px
    text-align: center
    background: #fff
    border: 1px solid #ddd
    border-radius: 10px
    box-shadow: 1px 5px 5px #999
    .alert-title
      width: calc(100% + 2px)
      height: 20px
      margin: -1px
      background: rgb(235, 35, 50)
      overflow: hidden
      i
        float: right
        height: 20px
        line-height: 20px
        margin-right: 3px
        color: #fff
    img
      margin-top: 10px
    span
      display: inline-block
      font-weight: 700
      margin-top: 10px
</style>
