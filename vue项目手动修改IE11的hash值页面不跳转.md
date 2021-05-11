
```js
// 方法一
if (!!window.ActiveXObject || 'ActiveXObject' in window) {
  window.addEventListener('hashchange', () => {
    let currentPath = window.location.hash.slice(1)
    if (this.$route.path !== currentPath) {
      this.$router.push(currentPath);// 主动更改路由界面
    }
  }, false);
}

// 方法二
const IE11RouterFix = {
  methods: {
    hashChangeHandler: function() { this.$router.push(window.location.hash.substring(1, window.location.hash.length)); },
    isIE11: function() { return !!window.MSInputMethodContext && !!document.documentMode; }
  },
  mounted: function() { if ( this.isIE11() ) { window.addEventListener('hashchange', this.hashChangeHandler); } },
  destroyed: function() { if ( this.isIE11() ) { window.removeEventListener('hashchange', this.hashChangeHandler); } }
};

new Vue({
  /* your stuff */
  mixins: [IE11RouterFix],
});
```