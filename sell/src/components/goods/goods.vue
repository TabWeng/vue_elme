<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li v-for="item in goods" class="menu-item">
          <span class="text border-1px">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="food-wrapper">
      <ul>
        <li v-for="item in goods" class="food-list">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item border-1px">
              <div class="icon">
                <img width="57" height="57" :src="food.icon">
              </div>
              <div class="content">
                <!--食品名称-->
                <h2 class="name">{{food.name}}</h2>
                <!--食品描述-->
                <p class="decs">{{food.description}}</p>
                <!--食品情况-->
                <div class="extra">
                  <span>月销{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <!--食品价格-->
                <div class="price">
                  <span>￥{{food.price}}</span>
                  <span v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">

  const ERR_OK = 0;

  export default{
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        goods:[]
      }
    },
    created() {
      this.classMap = ['decrease','discount','special','invoice','guarantee'];
      this.$http.get("api/goods").then((response) => {
        response = response.body;
        if(response.error === ERR_OK){
          this.goods = response.goods;
        }
      });
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">

  @import "../../common/stylus/mixin.styl"

  .goods
    display: flex
    position: absolute
    top: 174px
    bottom: 46px
    width: 100%
    overflow: hidden
    .menu-wrapper
      flex: 0 0 80px
      width: 80px
      background: #f3f5f7
      .menu-item
        display: table
        width: 54px
        height: 56px
        padding: 0 12px
        line-height: 14px
        .icon
          display: inline-block
          vertical-align: top
          width: 12px
          height: 12px
          margin-right: 2px
          background-size: 12px 12px
          background-repeat: no-repeat
          &.decrease
            bg-image('decrease_3')
          &.discount
            bg-image('discount_3')
          &.guarantee
            bg-image('guarantee_3')
          &.invoice
            bg-image('invoice_3')
          &.special
            bg-image('special_3')


        .text
          display: table-cell
          width: 56px
          vertical-align: middle
          border-1px(rgba(7, 17, 27, 0.1))
          font-size: 12px
    .food-wrapper
      flex: 1
      .title
        height: 26px
        line-height: 26px
        padding-left: 14px
        border-left: 2px solid #d9dde1
        background-color: #f3f5f7
        color: rgb(147, 153, 159)
        font-size: 12px

      .food-item
        display: flex
        margin: 18px
        padding-bottom: 18px
        border-1px(rgba(7, 17, 27, 0.1))
        &:after-child
          border-none()
          margin-bottom: 0
        .icon
          flex: 0 0 57px
          margin-right: 10px
          img
            border-radius: 1px


        .name
          margin: 2px 0 8px 0
          height: 14px
          line-height: 14px
          color: rgb(7, 17, 27)
          font-size: 14px
        .decs
          margin-bottom: 8px
          color: rgb(147, 153, 159)
          font-size: 10px
        .extra
          margin-bottom: 8px
          color: rgb(147, 153, 159)
          font-size: 10px
          &:first-child
            margin-left: 12px
        .price







</style>
