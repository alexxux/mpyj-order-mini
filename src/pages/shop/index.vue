<template>
  <div class="home-page">
    <swiper class="swiper-box">
      <swiper-item
       v-for="(item, index) in swiperList"
       :key="index"
       class="swiper-item"
      >
        <img :src="item.img" class="swiper-img">
      </swiper-item>
    </swiper>
    <div class="shop-menu">
      <scroll-view
       scroll-y
       class="menu-class"
      >
        <!-- 侧边栏 -->
        <ul class="menu-class-box">
          <li
           v-for="(c,index) in menuData"
           :key="index"
           @click="selectClass(index)"
           :class="{'class-selected': index === selectMenu}"
           class="class-item"
          >
            <div class="icon-box">
              <i class="svg-icon" :class="{'shop-class-hot': c.name==='热销商品', 'shop-class-dumpling': c.name==='饺子类' || c.name==='云吞类' || c.name==='包点类'}"></i>
            </div>
            <div class="class-name">{{c.name}}</div>
            <i :class="{'class-bar-selected': index === selectMenu}"></i>
          </li>
        </ul>
      </scroll-view>
      <!-- 菜单界面 -->
      <scroll-view
       scroll-y
       class="menu-list"
      >
        <ul class="menu-list-box">
          <li
           v-for="(item, index) in menuData[selectMenu].foods"
           :key="index"
           class="menu-item"
          >
            <div class="item-icon-box">
              <img :src="item.icon" class="item-icon">
          </div>
              <div class="item-content">
                <div class="item-text">
                  <div class="name">{{item.name}}</div>
                  <div class="description">{{item.description}}</div>
                </div>
                <div class="price-box">
                  <span>￥</span><span class="price">
                    {{item.price}}
                </span></div>
                <!-- 数量控制栏 start-->
                <div class="cartcontrol">
                  <div
                   class="cart-decrease iconfont icon-jianhao"
                   v-show="item.count>0"
                   @click.stop.prevent="decreaseCart(item)"
                  >
                  </div>
                  <div
                   class="cart-count"
                   v-show="item.count>0"
                  >{{item.count}}</div>
                  <div
                   class="cart-add iconfont icon-jiahao2fill"
                   @click.stop.prevent="addCart(item)"
                  ></div>
                </div>
                <!-- 数量控制栏 end-->
              </div>
          </li>
        </ul>
      </scroll-view>
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
            <span class="text" @click="handleSumbitCart">去结算</span>
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
          <div
           class="clear-button"
           @click="handleClearCart"
          >
            <i class="icon iconfont icon-lajixiang"></i>
            <span class="text">清空购物车</span>
          </div>
          <scroll-view
           scroll-y
           class="list-box"
           :class="{'list-box-isscroll': cartFoods.length>3}"
          >
            <div
             v-for="(food, index) in cartFoods"
             :key="index"
             class="list-item"
            >
              <span class="name">{{ food.name }}</span>
              <!-- 购物车数量控制栏 start-->
              <div class="cartcontrol">
                <div
                 class="cart-decrease iconfont icon-jianhao"
                 v-show="food.count>0"
                 @click.stop.prevent="decreaseCart(food)"
                >
                </div>
                <div
                 class="cart-count"
                 v-show="food.count>0"
                >{{food.count}}</div>
                <div
                 class="cart-add iconfont icon-jiahao2fill"
                 @click.stop.prevent="addCart(food)"
                ></div>
              </div>
              <!-- 购物车数量控制栏 end-->
            </div>
            <div
             v-if="cartCount===0"
             class="list-item"
            >购物车为空</div>
          </scroll-view>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import shopJson from 'static/mock/shop.json'
import { formatPrice } from '@/utils/index.js'
import Vue from 'vue'
export default {
  data() {
    return {
      menuData: [],
      selectMenu: 0,
      swiperList: [{
        name: 'img1',
        img: require('common/images/shop/shop-swiper-01.png')
      }],
      showCartList: false
    }
  },
  onLoad() {
    if (shopJson.code === 0) {
      this.menuData = shopJson.data
      //  格式化价格，末位补零
      this.menuData.forEach((list) => {
        list.foods.forEach((food) => {
          food.price = formatPrice(food.price)
        })
      })
    }
    console.log(this.menuData)
  },
  computed: {
    cartFoods() {
      let foods = []
      this.menuData.forEach((list) => {
        list.foods.forEach((item) => {
          if (item.count) {
            foods.push(item)
          }
        })
      })
      this.globalData.gcartFoods = foods
      return foods
    },
    cartCount() {
      let num = 0
      this.cartFoods.forEach((item) => {
        num += item.count
      })
      this.globalData.gcartCount = num
      return num
    }
  },
  methods: {
    selectClass(index) {
      this.selectMenu = index
    },
    handleClearCart() {
      this.menuData.forEach((list) => {
        list.foods.forEach((item) => {
          if (item.count) {
            Vue.set(item, 'count', 0)
          }
        })
      })
      this.handleCartList()
    },
    handleSumbitCart() {
      if (this.cartFoods.length < 1) {
        wx.showToast({
          title: '请选择菜品',
          icon: 'none',
          duration: 2000
        })
      } else {
        const url = '../orderbill/main'
        mpvue.navigateTo({ url })
      }
    },
    //  数量控制栏方法 start
    addCart(item) {
      if (!item.count) {
        Vue.set(item, 'count', 1)
      } else {
        item.count++
      }
    },
    decreaseCart(item) {
      if (item.count) {
        item.count--
      }
    },
    //  数量控制栏 end
    //  购物车栏 start
    handleCartList() {
      this.showCartList = !this.showCartList
    }
    //  购物车栏 end
  }
}

</script>
<style
 lang="scss"
 scoped
>
@import '~common/style/svg.scss';

.home-page {
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  overflow: hidden;
}

.swiper-box {
  width: 100%;
  height: 300rpx;

  .swiper-item {
    .swiper-img {
      width: 100%;
      height: 100%;
    }
  }
}

//  菜单组件样式
.shop-menu {
  .menu-class {
    position: absolute;
    top: 300rpx;
    left: 0;
    bottom: 0;
    width: 140rpx;
    background: #f7f7f7;
  }

  .menu-list {
    position: absolute;
    top: 300rpx;
    left: 140rpx;
    right: 0;
    bottom: 100rpx;
    background: #ffffff;
  }
}


.menu-class {
  .menu-class-box {
    .class-item {
      width: 140rpx;
      height: 140rpx;
      font-size: 30rpx;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;

      .icon-box {
        .svg-icon {
          width: 45px;
          height: 45px;
        }
      }

      .class-name {
        font-size: 12px;
      }
    }

    .class-selected {
      background: #ffffff;
      color: #3d5d98;
    }

    .class-bar-selected {
      box-sizing: border-box;
      border-left: 6rpx solid #3d5d98;
      position: absolute;
      width: 140rpx;
      height: 140rpx;
    }
  }
}

.menu-list {
  .menu-list-box {
    padding-bottom: 10px;

    .menu-item {
      box-sizing: border-box;
      width: 560rpx;
      height: 300rpx;
      margin: 15px;
      padding: 0 10px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      box-shadow: 0 0 5px 5px rgba(220, 220, 220, 0.5);
      border-radius: 3px;
      position: relative;

      .item-icon-box {
        width: 300rpx;
        height: 260rpx;

        .item-icon {
          max-width: 100%;
          max-height: 100%;
        }
      }

      .item-content {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        align-items: flex-end;
        height: 100%;
        position: relative;
        padding-top: 55rpx;
        padding-bottom: 30rpx;

        .item-text {
          text-align: right;

          .name {
            font-weight: 700;
            font-size: 28rpx;
          }

          .description {
            font-size: 10px;
          }
        }

        .price-box {
          font-size: 20px;
          color: #db5144;
          position: absolute;
          bottom: 100rpx;
          display: flex;
          align-items: flex-end;

          .price {
            font-size: 25px;
          }
        }
      }
    }
  }
}


//  数量控制栏样式
.cartcontrol {
  display: flex;
  justify-content: space-around;
  align-items: center;
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

//  购物车样式 start
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

//  购物车样式 end

</style>
