<template>
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
          <div>
            <i class="svgIcon" :class="{'shop-class-hot': c.name==='热销商品', 'shop-class-dumpling': c.name==='饺子类' || c.name==='云吞类' || c.name==='包点类'}"></i>
          </div>
          <div>{{c.name}}</div>
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
                <span>￥</span><span class="price">{{item.price}}</span></div>
              <cartcontrol :food="item"></cartcontrol>
            </div>
        </li>
      </ul>
    </scroll-view>
  </div>
</template>
<script>
import cartcontrol from 'components/cartcontrol'
export default {
  props: {
    menuData: Object
  },
  data() {
    return {
      selectMenu: 0,
      classIcon: {

      }
    }
  },
  components: {
    cartcontrol
  },
  methods: {
    selectClass(index) {
      this.selectMenu = index
    },
    iconSvg(name) {
      return 'shop-class-hot'
    }
  }
}

</script>
<style
  lang="scss"
  scoped
>
@import '~common/style/svg.scss';

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
    bottom: 0;
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
    }
    .class-selected {
      background: #ffffff;
    }
  }
}

.menu-list {
  .menu-list-box {
    .menu-item {
      box-sizing: border-box;
      width: 560rpx;
      height: 300rpx;
      margin: 15px;
      padding: 0 10px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      box-shadow: 0 0 2px 2px rgba(220, 220, 220, 0.5);
      border-radius: 3x;

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
        height: 80%;

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

        }
      }
    }
  }
}

</style>
