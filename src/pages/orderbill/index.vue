<template>
  <form class="order-bill" @submit="handleSubmit">
    <div class="shop">
      <div class="shop-name">
        <span>蛇口店</span>
        <i class="iconfont icon-z043"></i>
      </div>
      <div class="shop-info">距您步行66米，请确认下单门店是否正确</div>
    </div>
    <radio-group @change="radioChange">
      <label
        v-for="radio in radioList"
        :key="radio.value"
      >
        <div
          class="radio-item"
          :class="{'radio-active': radio.checked}"
        >
          <radio
            :value="radio.value"
            :checked="radio.checked"
            name="orderMode"
          ></radio>
          <i class="iconfont" :class="{'icon-baozhuang': radio.value==='takeoutex', 'icon-waimaixinxi': radio.value ==='takeout'}"></i>
          {{radio.name}}
        </div>
      </label>
    </radio-group>
    <div class="message-box">
      <picker
        @change="pickerChange"
        mode="time"
        :value="selectedTime"
      >
      <span>预计送达时间</span>
        <view class="picker">
          {{selectedTime}}
        </view>
      </picker>
      <div class="phoneBox">
        <span>预留电话</span>
        <span></span>
      </div>
    </div>
    <button formType="submit">提交</button>
  </form>
</template>
<script>
export default {
  data() {
    return {
      radioList: [{
        name: '打包外带',
        value: 'takeoutex',
        checked: 'true'
      }, {
        name: '外卖配送',
        value: 'takeout'
      }],
      selectedTime: '12:22'
    }
  },
  mounted() {
    this.selectedTime = this.getNowTime()
  },
  methods: {
    getNowTime() {
      return new Date().getHours().toString() + ':' + new Date().getMinutes().toString()
    },
    radioChange(e) {
      console.log(e.target.value)
    },
    pickerChange(e) {
      this.selectedTime = e.mp.detail.value
    },
    handleSubmit(e) {
      console.log(e)
    }
  }
}

</script>
<style
  lang="scss"
  scoped
>
radio {
  opacity: 0;
}

</style>
