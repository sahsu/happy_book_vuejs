# 底部Tab

頁面的底部Tab是非常重要的部分， 哪個頁面都會用到。 

下面就是使用的步驟

## 1. 在首頁中引用底部Tab

```
<template>
  <div class="background">
    <div class="home">
      <div class="m_layout">
        <!-- 輪播圖-->
        <HomeBannerView></HomeBannerView>
      </div>
    </div>
    <!-- 這裏就是底部Tab -->
    <NavBottomView :is_shops_index="is_shops_index"></NavBottomView>
  </div>
</template>
<script>
    // 這裏就是底部Tab對應的vue文件
    import NavBottomView from '../../components/NavBottom.vue';
</script>
```

## 2. 增加對應的component文件

增加 /components/NavBottom.vue ：

```
<template>
  <div class="footer">
  	<footer class="fixBottomBox">
        <ul>
            <router-link tag="li" to="/" class="tabItem">
              <a href="javascript:;" class="tab-item-link" v-if="is_shops_index">
                <img src="../assets/footer01.png" alt="" class="tabbar-logo">
                <p class="tabbar-text" style="color: rgba(234, 49, 6, 0.66);">首頁</p>
              </a>
              <a href="javascript:;" class="tab-item-link" else>
                <img src="../assets/footer001.png" alt="" class="tabbar-logo">
                <p class="tabbar-text">首頁</p>
              </a>
            </router-link>
            <router-link tag="li" to="/shops/category" class="tabItem">
                <a href="javascript:;" class="tab-item-link" v-if="is_category">
                    <img src="../assets/footer02.png" alt="" class="tabbar-logo">
                    <p class="tabbar-text" style="color: rgba(234, 49, 6, 0.66);">分類</p>
                </a>
                <a href="javascript:;" class="tab-item-link" else>
                  <img src="../assets/footer002.png" alt="" class="tabbar-logo">
                  <p class="tabbar-text">分類</p>
                </a>
            </router-link>
            <router-link tag="li" to="/cart" class="tabItem">
                <a href="javascript:;" class="tab-item-link" v-if="is_cart">
                  <img src="../assets/footer03.png" alt="" class="tabbar-logo">
                  <p class="tabbar-text" style="color: rgba(234, 49, 6, 0.66);">購物車</p>
                </a>
                <a href="javascript:;" class="tab-item-link" else>
                  <img src="../assets/footer003.png" alt="" class="tabbar-logo">
                  <p class="tabbar-text">購物車</p>
                </a>
            </router-link>
            <router-link tag="li" to="/mine" class="tabItem">
                <a href="javascript:;" class="tab-item-link" v-if="is_mine">
                  <img src="../assets/footer04.png" alt="" class="tabbar-logo">
                  <p class="tabbar-text" style="color: rgba(234, 49, 6, 0.66);">我的</p>
                </a>
                <a href="javascript:;" class="tab-item-link" else>
                  <img src="../assets/footer004.png" alt="" class="tabbar-logo">
                  <p class="tabbar-text">我的</p>
                </a>
            </router-link>
        </ul>
    </footer>
  </div>
</template>

<script>
export default{
  data () {
    return {
    }
  },
  props: {
    is_shops_index: Boolean,
    is_category: Boolean,
    is_cart: Boolean,
    is_mine: Boolean,
  },
  mounted () {
  },
  computed: {
  },
  methods: {
  }
}
</script>
```

## 效果圖

效果如下圖所示：

![底部Tab](/images/real_project/bottom_tab.png)

## 總結

底部Tab 很簡單，又很重要。 

在本節中，我們使用了一個component來實現它，然後在所有用到它的頁面來使用。 是一個非常典型的重用過程。

