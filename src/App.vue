<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/sellers">商家</router-link>
      </div>
    </div>
    <router-view></router-view>
  </div>
</template>

<script>
import axios from 'axios';
import header from './components/header/header';
import { urlParse } from './common/js/util';

const ERR_OK = 0;

export default {
  data() {
    return {
      seller: {
        id: (() => {
          const queryParam = urlParse();
          return queryParam.id;
        })(),
      },
    };
  },
  created() {
    axios.get('/api/seller').then((response) => {
      const res = response.data;
      if (res.errno === ERR_OK) {
        this.seller = Object.assign({}, this.seller, res.data);
      }
    });
  },
  components: {
    'v-header': header,
  },
};
</script>

<style lang="scss" rel="stylesheet/scss">
@import 'common/scss/mixin.scss';
#app .tab {
  display: flex;
  width: 100%;
  height: 40px;
  line-height: 40px;
  @include border-1px(rgba(7, 17, 27, 0.1));
  .tab-item {
    flex: 1;
    text-align: center;
    a {
      display: block;
      font-size: 14px;
      color: rgb(77, 85, 93);
      &.active {
        color: rgb(240, 20, 20);
      }
    }
  }
}
</style>
