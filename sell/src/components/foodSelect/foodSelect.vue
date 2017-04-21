<template>

  <div class="select">
    <div class="subtract" v-show="food.count > 0" @click="subtractSelect" transition="move">
      <span class="inner icon-remove_circle_outline"></span>
    </div>
    <div class="count" v-show="food.count > 0">{{food.count}}</div>
    <div class="add icon-add_circle" @click="addSelect"></div>
  </div>

</template>

<script type="text/ecmascript-6">

  import Vue from 'vue';

  export default {
    props:{
      food:{
        type: Object
      }
    },
    methods: {
      addSelect(event){
        if(!event._constructed){
          return;
        }
        if(!this.food.count){
          Vue.set(this.food,'count', 1);
        }else{
          this.food.count++;
        }
        this.$dispatch('car_add', event.target);
      },
      subtractSelect(event){
        if(!event._constructed){
          return;
        }
        this.food.count--;
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .select
    font-size: 0
    .subtract,.add
      display: inline-block
      vertical-align: top
      padding: 6px
      color: rgb(0, 160, 220)
    .subtract
      transition: all 0.3s linear
      &.move-transition
        opacity: 1
        transform: translate3d(0, 0, 0)
        .inner
          display: inline-block
          line-height: 24px
          font-size: 24px
          transition: all 0.3s linear
          transform: rotate(0)
      &.move-enter, &.move-leave
        opacity: 0
        transform: translate3d(24px, 0, 0)
        .inner
          transform: rotate(180deg)
    .add
      line-height: 24px
      font-size: 24px
    .count
      display: inline-block
      width:  12px
      height: 36px
      line-height: 36px
      text-align: center
      font-size: 10px
      color: rgb(147, 153, 159)

</style>
