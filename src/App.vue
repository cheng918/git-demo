<template>
  <div id="app">
    <!--头部组件-->
    <v-header :seller="seller"></v-header>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/rating">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <!--我们可以通过路由去传递一些对象信息  router切换我们的组件状态发生改变 我们使用keep-alive保留组件状态 保留在内存中 需要的时候再恢复-->
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script>
  import header from './components/header/header.vue'
  import {urlParse} from './assets/js/util'

  const ERR_OK = 0;
  export default {
    name: 'app',
    data() {
      return {
          s:{},
        //一开始我们的seller 应该是有参数的 而这个参数就是根据url获取到的  我们收藏要用 我们去拿一下这个id
        seller: {
          id: (() => {
            let queryParam = urlParse();
            //console.log(queryParam.id);
            return queryParam.id;
          })()
        }
      };
    },
    mounted(){
      console.log(1)
      console.log(this.seller.id)
      this.$http.get('/api/seller?id='+this.seller.id).then((res) =>  {
        var response = res.data;
        if (response.errno === ERR_OK) {
          console.log(response.data)
          this.seller = response.data;
          console.log(response.data)
        }
      }, function () {

      })
      /*
       this.$http.get('/api/json/sell.json?id=' + this.seller.id).then((res) => {
       //            console.log(res.data.goods)
       if (res.data.errno === ERR_OK) {
       //          this.seller = res.data.seller;
       //           console.log(this.goods)
       //          这里有一个方法 支持三个参数 给对象拓展属性的一个方法 最终要接受的一个结果
       this.seller = Object.assign({}, this.seller, res.data.seller);
       console.log(this.seller.id)  //未定义的 我们添加的id属性被干掉了
       }
       });
       */
    },
    components: {
      'v-header': header
    }
  }
</script>

<style lang="less" scoped>

  @import "assets/style/main.less";

  #app {
    .tab {
      display: flex;
      width: 100%;
      .h(40);
      .lh(40);
      border-bottom: 1px solid rgba(7, 17, 27, .1);
      .tab-item {
        flex: 1;
        text-align: center;
        /*& 表示他的父元素 这里指tab-item*/
        & > a {
          /*自动充满了盒子*/
          display: block;
          .fs(14);

          color: rgb(77, 85, 93);
          &.router-link-active {
            color: rgb(240, 20, 20);
          }
        }
      }
    }
  }

</style>
