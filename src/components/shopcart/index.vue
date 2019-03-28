<template>
  <div class="shop-cart">
    <!-- 购物车底栏 -->
    <div class="cart-bar">
      <div
       class="cart-button"
       @click="handleCartList"
      >
        <i class="cart-icon iconfont icon-gouwuche"></i>
        <span class="cart-text">购物车</span>
        <i class="cart-count" v-show="cartCount">{{cartCount}}</i>
      </div>
      <div class="submit-button">
        <span class="text" @click="handleCartSumbit">去结算</span>
      </div>
    </div>
    <!-- 购物车列表遮罩 -->
    <div
     class="cart-shade"
     v-show="showCartList"
     @click="handleCartList"
    ></div>
    <!-- 购物车列表 -->
    <div
     class="cart-list"
     v-show="showCartList"
    >
      <div class="clear-button" @click="handleClear">
        <i class="icon iconfont icon-lajixiang"></i>
        <span class="text">清空购物车</span>
      </div>
      <scroll-view
       scroll-y
       class="list-box"
       :class="{'list-box-isscroll': cartData.length>3}"
      >
        <div
         v-for="(food, index) in cartData"
         :key="index"
         class="list-item"
        >
          <span class="name">{{ food.name }}</span>
          <cartcontrol :food="food"></cartcontrol>
        </div>
        <div
         v-if="cartCount===0"
         class="list-item"
        >购物车为空</div>
      </scroll-view>
    </div>
  </div>
</template>
<script>
import cartcontrol from 'components/cartcontrol'
export default {
  props: {
    cartData: Object
  },
  data() {
    return {
      showCartList: false
    }
  },
  computed: {
    cartCount() {
      let num = 0
      this.cartData.forEach((item) => {
        num += item.count
      })
      return num
    }
  },
  components: {
    cartcontrol
  },
  methods: {
    handleCartList() {
      this.showCartList = !this.showCartList
    },
    handleClear() {
      this.$emit('clearcart')
      this.handleCartList()
    },
    handleCartSumbit() {
      this.$emit('sumbitcart')
    }
  }
}

</script>
<style
 lang="scss"
 scoped
>
.shop-cart {
  width: 100%;
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
}

.cart-bar {
  height: 100rpx;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #f8f8f8;

  .cart-button {
    position: relative;
    margin-left: 55rpx;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 170rpx;
    height: 65rpx;
    background: #ffffff;
    border-radius: 20px;

    .cart-icon {
      font-size: 25px;
    }

    .cart-text {
      font-size: 13px;
      margin-left: 3px;
    }

    .cart-count {
      position: absolute;
      top: -5px;
      right: 0;
      background: #3d5d98;
      width: 15px;
      height: 15px;
      border-radius: 15px;
      color: #ffffff;
      text-align: center;
      font-size: 12px;
    }
  }

  .submit-button {
    width: 200rpx;
    height: 100rpx;
    background: #3d5d98;
    color: #ffffff;

    .text {
      position: absolute;
      line-height: 100rpx;
      right: 45rpx;
    }
  }
}

.cart-list {
  position: absolute;
  bottom: 100rpx;
  left: 0;
  right: 0;
  background: #ffffff;

  .clear-button {
    width: 100%;
    height: 60rpx;
    background: #f8f9f9;
    display: flex;
    align-items: center;
    color: #8e8e8e;
    padding-left: 20rpx;
    font-size: 13px;

    .text {
      padding-left: 16rpx;
    }
  }
.list-box {
  .list-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 28rpx 0 52rpx;
    height: 140rpx;
    border-bottom: 1px solid #f8f8f8;
  }
}
.list-box-isscroll {
  height: 420rpx;
}
}

.cart-shade {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 100rpx;
  background: rgba(0, 0, 0, 0.5);
}

</style>
