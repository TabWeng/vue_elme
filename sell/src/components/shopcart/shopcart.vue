<template>

  <div class="shopcart">
    <div class="content">
      <div class="content-left">
        <div class="logo-wrapper">
          <i class="icon-shopping_cart" :class="{'highlight':totalCount > 0}"></i>
          <span class="num" v-show="totalCount > 0">{{totalCount}}</span>
        </div>
        <span class="price" :class="{'highlight':totalCount > 0}">￥{{totalPrice}}</span>
        <span class="desc">另需配送费￥{{deliveryPrice}}元</span>
      </div>
      <div class="content-right" :class="setGoClass">
        <span class="sending-price">{{payDesc}}</span>
      </div>
      <!--动画的小球-->
      <div class="balls-container">
        <div class="balls" v-for="ball in balls" v-show="ball.show" transition="drop">
          <div class="inner inner-hook"></div>
        </div>
      </div>

    </div>
  </div>

</template>

<script type="text/ecmascript-6">
  export default{
    props: {
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      },
      selectFoods: {
        type: Array,
        default(){
          return [];
        }
      }
    },
    data() {
      return {
        balls:[{show:false},{show:false},{show:false},{show:false},{show:false}],
        dropBalls:[]
      }
    },
    computed: {
      totalPrice(){
        let total = 0;
        this.selectFoods.forEach((food) => {
          total += food.price * food.count;
        });
        return total;
      },
      totalCount(){
        let count = 0;
        this.selectFoods.forEach((food) => {
          count += food.count;
        });
        return count;
      },
      payDesc(){
        let totalPrice = this.totalPrice;
        let minPrice = this.minPrice;
        let diffPrice = minPrice - totalPrice;
        if(totalPrice === 0){
          return `￥${minPrice}起送`;
        }else if(totalPrice < minPrice){
          return `还差￥${diffPrice}起送`
        }else{
          return '去结算';
        }
      },
      setGoClass(){
        if(this.totalPrice >= this.minPrice){
          return "go"
        }
      }
    },
    methods: {
      drop(el){
        for (let i = 0; i < this.balls.length; i++){
          let ball = this.balls[i];
          if (!ball.show){
            ball.show = true;
            ball.el = el;
            this.dropBalls.push(ball);
            return;
          }
        }
      }
    },
    transitions: {
      drop: {
        beforeEnter(el) {
          let count = this.balls.length;
          while(count--){
            let ball = this.balls[count];
            if (ball.show) {
              let rect = ball.el.getBoundingClientRect();
              let x = rect.left - 32;
              let y = -(window.innerHeight - rect.top - 44);
              el.style.display = '';
              el.style.webkitTransform = `translate3d(0, ${y}px, 0)`;
              el.style.transform = `translate3d(0, ${y}px, 0)`;
              let inner = el.getElementsByClassName('inner-hook')[0];
              inner.style.webkitTransform = `translate3d(${x}px, 0, 0)`;
              inner.style.transiform = `translate3d(${x}px, 0, 0)`;
            }
          }
        },
        enter(el) {
          /* eslint-disable no-unused-vars */
          let rf = el.offsetHeight;
          this.$nextTick(() => {
            el.style.webkitTransform = 'translate3d(0,0,0)';
            el.style.transform = 'translate3d(0,0,0)';
            let inner = el.getElementsByClassName('inner-hook')[0];
            inner.style.webkitTransform = 'translate3d(0,0,0)';
            inner.style.transform = 'translate3d(0,0,0)';
          });
        },
        afterEnter(el) {
          let ball = this.dropBalls.shift();
          if (ball) {
            ball.show = false;
            el.style.display = 'none';
          }
        }
      }
    }

  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .shopcart
    position: fixed
    left: 0
    bottom: 0
    z-index: 20
    width: 100%
    height: 48px
    background: #141d27
    .content
      display: flex
      line-height: 24px
      .content-left
        flex: 1
        font-size: 0
        color: rgb(128, 133, 138)
        .logo-wrapper
          display: inline-block
          vertical-align: top
          position: relative
          top: -10px
          margin: 0 15px
          width: 44px
          height: 44px
          padding: 6px
          text-align: center
          border-radius: 50%
          background: #141d27
          font-size: 24px
          .icon-shopping_cart
            display: inline-block
            vertical-align: top
            width: 44px
            height: 44px
            line-height: 44px
            box-sizing: border-box
            border-radius: 50%
            background: rgb(43, 52, 60)
            font-size: 24px
            &.highlight
              color: #fff
              background: rgb(0, 160, 220)
           .num
            display: inline-block
            position: absolute
            left: 29px
            top: 0px
            height: 16px
            line-height: 16px
            width: 24px
            text-align: center
            border-radius: 16px
            box-shadow: 0px 4px 8px 0px rgba(0,0,0,0.4)
            background: rgb(240, 20, 20)
            font-weight: 700
            font-size: 9px
            color: rgb(255, 255, 255)
        .price
          display: inline-block
          vertical-align: top
          padding-right: 12px
          margin: 12px 0
          border-right: 1px solid rgb(128, 133, 138)
          font-size: 16px
          font-weight: 700
          &.highlight
            color: #fff
        .desc
          display: inline-block
          line-height: 48px
          line-height: 48px
          margin-left: 12px
          margin-right: 0
          font-weight: 700
          font-size: 12px
      .content-right
        flex: 0 0 105px
        width: 105px
        text-align: center
        background: rgb(43, 51, 59)
        &.go
          background: #00b43c
          .sending-price
            color: #fff
        .sending-price
          display: inline-block
          height: 48px
          line-height: 48px
          font-size: 12px
          font-weight: 700
          color: rgb(128, 133, 138)


      .balls-container
        .balls
          position: fixed
          left: 32px
          bottom: 22px
          &.drop-transition
            transition: all 0.4s cubic-bezier(.22,-0.37,.8,.3)
            .inner
              width: 16px
              height: 16px
              border-radius: 50%
              background: rgb(0, 160, 220)
              transition: all 0.4s linear
</style>
