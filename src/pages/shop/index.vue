<template>
  <div class="home-page">
    <swiper class="swiper-box">
      <swiper-item v-for="(item, index) in swiperList" :key="index" class="swiper-item">
        <img :src="item.img" class="swiper-img">
      </swiper-item>
    </swiper>
    <shop-menu :menuData="menuData"></shop-menu>
  </div>
</template>
<script>
import shopJson from 'static/mock/shop.json'
import shopMenu from 'components/shop-menu'
export default {
  data() {
    return {
      menuData: [],
      swiperList: [{
        name: 'img1',
        img: require('common/images/shop/shop-swiper-01.png')
      }]
    }
  },
  components: {
    shopMenu
  },
  onLoad() {
    if (shopJson.code === 0) {
      this.menuData = shopJson.data
    }
    console.log(this.menuData)
  }
}

</script>
<style
 lang="scss"
 scoped
>
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
</style>
