## version 4 ~ 7

vue-awesome-swiper:
```html
<!-- 用v-if控制loop环状轮播，否则不起作用：https://blog.csdn.net/qinyongqaq/article/details/82787707-->
<swiper v-if="data.length"></swiper>
```


```js
{
  // 根据slide的宽度自动调整展示数量。此时需要设置slide的宽度
  slidesPerView: 'auto',
}
```

```css
/* 修复轮播按钮在容器外部时有选中状态 */
.swiper-button-prev,
.swiper-button-next {
  outline: none !important;
}
```