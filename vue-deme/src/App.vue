<template>
  <div id="app">
    <v-header v-bind:seller="seller"></v-header>
    <div class="tac">
        <div class="tab-item">
          <router-link :to="{name:'Goods',params:{goods:goods}}">商品</router-link>
        </div>
        <div class="tab-item">
          <router-link to="/ratings">评价</router-link>
        </div>
         <div class="tab-item">
          <router-link to="/seller">商家</router-link>
        </div>
    </div>
      <router-view :seller="seller"/>
  </div>
</template>
<script>
import header from './components/header/header.vue'
import axios from 'axios'
//var appData = require('../data.json');
export default {
  name: 'App',
  data() {
    return{
      seller:{},
      goods:{},
      ratings:{}
    }
  },
  created(){
        this.$http.get('http://localhost:8080/static/data.json').then(function res(e){
          this.seller = e.body.seller;
          this.goods = e.body.goods;
          this.ratings = e.body.ratings;
        });/*.then(function ree(){
          console.log( this.goods);
        });*/
        /*axios({
          method:'get',
          url:'http://localhost:8080/static/data.json'
        }).then(function res(rw){
          console.log(rw);
        }).catch(function ree(er){
          console.log(er);
        });*/
  },
  /*mounted(){
    this.$router.go('/goods');
  },*/
  components:{
      'v-header':header
  }
}
</script>
  
<style scoped lang="less">
#app {
  .tac{
    display:flex;
    width:100%;
    height:40px;
    line-height:40px;
    border-bottom:0.5px solid rgba(7,17,27,0.1); 
    .tab-item{
      flex:1;
      text-align:center;
      a{
        display: block;
        font-size: 14px;
        color: rgb(77,85,93);
      }
      .router-link-active{
        color: rgb(240,20,20);
      }
    }
  }
}

</style>
