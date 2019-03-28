<template>
  <form class="order-bill">
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
        <div class="radio-item" :class="{'radio-active': radio.checked}">
          <radio
            :value="radio.value"
            :checked="radio.checked"
          ></radio>
          <i class="iconfont" :class="{'icon-baozhuang': radio.value==='takeoutex', 'icon-waimaixinxi': radio.value ==='takeout'}"></i>
          {{radio.name}}
        </div>
      </label>
    </radio-group>
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
      }]
    }
  },
  methods: {
    radioChange(e) {
      console.log(e.target.value)
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
