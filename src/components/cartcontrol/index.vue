<template>
  <div class="cartcontrol">
    <div class="cart-decrease iconfont icon-jianhao" v-show="food.count>0" @click.stop.prevent="decreaseCart">
    </div>
    <div
     class="cart-count"
     v-show="food.count>0"
    >{{food.count}}</div>
    <div
     class="cart-add iconfont icon-jiahao2fill"
     @click.stop.prevent="addCart"
    ></div>
  </div>
</template>
<script>
import Vue from 'vue'

export default {
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    addCart(event) {
      if (!this.food.count) {
        Vue.set(this.food, 'count', 1)
      } else {
        this.food.count++
      }
      this.$emit('add', event.target)
    },
    decreaseCart(event) {
      if (this.food.count) {
        this.food.count--
      }
    }
  }
}
</script>
<style lang="scss">
.cartcontrol {
  display:flex;
  justify-content:space-around;
  align-items:center;
  color: #3d5d98;
  height: 20px;
  .cart-decrease {
    font-size: 15px;
  }
  .cart-count {
    font-size: 15px;
    margin: 0 10px;
  }
  .cart-add {
    font-size: 15px;

  }
}
</style>
