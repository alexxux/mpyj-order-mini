<template>
  <form
   class="order-bill"
   @submit="handleSubmit"
  >
    <scroll-view
     scroll-y
     class="scroll-view"
    >
      <div class="shop">
        <div class="shop-name">
          <span>选择收货地址</span>
          <i class="iconfont icon-z043"></i>
        </div>
        <div class="shop-info">距您步行66米，请确认下单门店是否正确</div>
      </div>
      <!-- 模式选择 -->
      <radio-group
       @change="radioChange"
       name="orderMode"
       class="order-mode"
      >
        <label
         v-for="radio in radioList"
         :key="radio.value"
         class="mode-item-box"
        >
          <radio
           :value="radio.value"
           :checked="orderMode === radio.value"
           class="mode-radio-item"
           hidden="true"
          ></radio>
          <div class="mode-item">
            <i class="icon iconfont" :class="{'icon-baozhuang': radio.value==='takeoutex', 'icon-waimaixinxi': radio.value ==='takeout'}"></i>
            <span>{{radio.name}}</span>
          </div>
        </label>
      </radio-group>
      <div class="contact-box">
        <!-- 时间选择 -->
        <picker
         @change="pickerChange"
         mode="time"
         :value="subdata.selectedTime"
         name="sendTime"
        >
          <div class="picker-box">
            <div class="text">
              <span v-if="orderMode === 'takeoutex'">自取时间</span>
              <span v-if="orderMode === 'takeout'">预计送达时间</span>
              <view class="picker-time">
                {{subdata.selectedTime}}
              </view>
            </div>
            <i class="iconfont icon-z043"></i>
          </div>
        </picker>
        <div class="phoneBox">
          <div class="text">
            <div>预留电话</div>
            <div
             @click="handleChangePhone"
             class="phone"
            >{{ subdata.phoneNumber }}</div>
          </div>
          <i class="iconfont icon-z043"></i>
        </div>
      </div>
      <!-- 已选商品 -->
      <div class="cart-foods item-box">
        <div class="box-title">已选商品：共{{ cartCount }}份</div>
        <ul class="foods-box">
          <li
           class="foods-item"
           v-for="(food, index) in cartFoods"
           :key="index"
          >
            <div class="foods-content">
              <img :src="food.icon" class="icon">
              <span class="text">{{food.name}} x {{ food.count }}</span>
            </div>
            <span>￥{{ food.price * food.count }}</span>
          </li>
        </ul>
        <div
         class="back-menu"
         @click="handleMenuBack"
        >
          <i class="iconfont icon-jiantou"></i>返回菜单</div>
      </div>
      <!--支付方式-->
      <div class="pay-mode item-box">
        <div class="box-title">请选择支付方式</div>
        <radio-group
         @change="payRadioChange"
         name="payMode"
         class="pay-mode-box"
        >
          <label
           v-for="pay in payList"
           :key="pay.value"
           class="mode-item"
          >
            <radio
             :value="pay.value"
             class="mode-radio-item item"
             :checked="pay.checked"
            ></radio>
            <i class="iconfont item" :class="{'icon-qianbao': pay.value === 'memberCard', 'icon-weixin': pay.value === 'wxpay'}"></i>
            <span class="item">{{pay.name}}</span>
          </label>
        </radio-group>
      </div>
      <!-- 会员券 -->
      <div class="member-coupon item-box">
        <span class="box-title">会员礼券</span>
        <i class="iconfont icon-z043"></i>
      </div>
      <!-- 备注 -->
      <div class="order-remarks item-box">
        <div class="box-title">备注信息</div>
        <textarea
         type="text"
         placeholder="请输入最多50字的备注信息"
         auto-height
         name="remarks"
         class="remarks"
        ></textarea>
      </div>
      <!-- 订单总计 -->
      <div class="order-info item-box">
        <div class="box-title">订单总计</div>
        <div class="sum-info">
          <div class="sum-item">
            <span>菜品小计</span>
            <span>￥{{sumbill}}</span>
          </div>
          <div class="sum-item">
            <span>优惠折扣</span>
            <span>-￥{{sumdiscount}}</span>
          </div>
          <div class="sum-item">
            <span>应付金额</span>
            <span>￥{{ sumpaid }}</span>
          </div>
        </div>
      </div>
    </scroll-view>
    <div class="submit-bar">
      <div class="order-price">
        <span>￥</span>
        <span>{{sumpaid}}</span>
      </div>
      <label class="button">
        <button
         formType="submit"
         hidden="true"
        ></button>
        <span>去支付</span>
      </label>
    </div>
  </form>
</template>
<script>
export default {
  props: {
    cartData: Object
  },
  data() {
    return {
      cartCount: 0,
      cartFoods: [],
      orderMode: 'takeoutex',
      radioList: [{
        name: '打包外带',
        value: 'takeoutex',
        checked: 'true'
      }, {
        name: '外卖配送',
        value: 'takeout'
      }],
      payList: [{
        name: '钱包支付',
        value: 'memberCard',
        checked: 'true'
      }, {
        name: '微信支付',
        value: 'wxpay'
      }],
      sumdiscount: 0,
      subdata: {
        selectedTime: '12:24',
        phoneNumber: 13555565525
      }
    }
  },
  mounted() {
    this.subdata.selectedTime = this.getNowTime()
    this.cartFoods = this.globalData.gcartFoods
    this.cartCount = this.globalData.gcartCount
  },
  methods: {
    getNowTime() {
      //  bug
      return new Date().getHours().toString() + ':' + new Date().getMinutes().toString()
    },
    radioChange(e) {
      this.orderMode = e.target.value
    },
    pickerChange(e) {
      console.log(e)
      this.subdata.selectedTime = e.mp.detail.value
    },
    handleSubmit(e) {
      let formData = e.mp.detail.value
      let obj = {
        foods: this.cartFoods,
        sumbill: this.sumbill,
        sumdiscount: this.sumdiscount,
        sumpaid: this.sumpaid
      }
      let data = Object.assign(formData, obj)
      console.log(data)
    },
    handleMenuBack() {
      wx.navigateBack({
        delta: 1
      })
    }
  },
  computed: {
    sumbill() {
      let num = 0
      this.cartFoods.forEach((food) => {
        if (food.count) {
          num += food.price * food.count
        }
      })
      return num.toFixed(2)
    },
    sumpaid() {
      return this.sumbill - this.sumdiscount
    }
  }
}

</script>
<style
 lang="scss"
 scoped
>
$theblue: #3d5d98;

.item-box {
  padding-top: 20rpx;
  padding-left: 40rpx;
  padding-right: 40rpx;
  background: #fff;
}

.box-title {
  font-weight: 600;
  font-size: 30rpx;
}

.graytext {
  color: #8a8a8a;
}

.scroll-view {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 100rpx;
  background: #f7f7f7;
}

.order-bill {
  .shop {
    padding-top: 30rpx;
    padding-left: 50rpx;
    background: #fff;

    .shop-name {
      display: flex;
      justify-conent: center;
      align-items: center;
      font-size: 36rpx;
      margin-right: 35rpx;
    }

    .shop-info {
      font-size: 25rpx;
      color: #6d6d6d;
      margin-top: 20rpx;
    }
  }

  .order-mode {
    display: flex;
    padding-top: 22rpx;
    padding-left: 48rpx;
    display: flex;
    background: #fff;

    .mode-item-box {
      width: 100%;

      .mode-item {
        display: flex;
        align-items: center;
        justify-content: space-around;
        width: 300rpx;
        height: 100rpx;
        box-shadow: 0 0 2px 2px rgba(220, 220, 220, 0.5);
        border-radius: 5px;

        radio {
          position: absolute;
          opacity: 1;
        }

        .icon {
          font-size: 45rpx;
          font-weight: 600;
        }
      }

      [Checked]+.mode-item {
        color: $theblue;
        box-sizing: content-box;
        border: 1px solid #3d5d98;
        box-shadow: 0 0 0px 0px rgba(220, 220, 220, 0.5);
      }
    }
  }

  .contact-box {
    background: #fff;
    padding-top: 45rpx;
    padding-left: 50rpx;
    padding-right: 70rpx;
    padding-bottom: 25rpx;
    with: 100%;
    display: flex;
    justify-content: space-between;

    .picker-box {
      display: flex;
      width: 240rpx;
      align-items: center;
      justify-content: space-between;
      color: #7f7f7f;
      font-size: 26rpx;

      .text {
        .picker-time {
          color: black;
        }
      }
    }
  }

  .phoneBox {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 26rpx;
    color: #7f7f7f;
    width: 240rpx;

    .phone {
      color: black;
    }
  }

  .cart-foods {
    margin-top: 20rpx;
    font-size: 35rpx;
    .foods-item {
      display: flex;
      justify-content: space-between;
      font-size: 28rpx;
      height: 175rpx;
      line-height: 175rpx;
      border-bottom: 1px solid #c0c0c0;

      .foods-content {
        display: flex;
        align-items: center;

        .icon {
          width: 130rpx;
          height: 130rpx;
          margin-right: 20rpx;
        }
      }
    }

    .foods-box {
      margin-top: 10rpx;
    }

    .back-menu {
      height: 110rpx;
      color: $theblue;
      display: flex;
      align-items: center;

      .iconfont {
        padding: 0 25rpx;
      }
    }
  }
}

.pay-mode {
  margin-top: 20rpx;

  .pay-mode-box {
    .mode-item {
      display: flex;
      align-items: center;
      height: 125rpx;

      .item {
        padding: 0 25rpx;
      }

      .iconfont {
        font-size: 50rpx;
      }
    }
  }
}

.member-coupon {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid #c0c0c0;
  height: 130rpx;

}

.order-remarks {
  padding-bottom: 50rpx;

  .box-title {
    padding-bottom: 20rpx;
  }
}

.order-info {
  .box-title {
    height: 80rpx;
    border-bottom: 1px solid #c0c0c0;
  }

  .sum-info {
    .sum-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 20rpx 0;
    }
  }
}

.submit-bar {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  height: 100rpx;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #ffffff;
  padding-left: 40rpx;

  .button {
    width: 200rpx;
    height: 100rpx;
    display: flex;
    justify-content: center;
    align-items: center;
    background: #4061a0;
    color: #fff;
    font-weight: 600;
  }
}

</style>
