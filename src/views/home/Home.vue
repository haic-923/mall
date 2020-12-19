<template>
<div id="home">
 <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
 <div>
 <home-swiper :banners="banners"></home-swiper>
 <recommend-view :recommends="recommends"></recommend-view>
 <feature-view></feature-view>
 <tab-control class="tab-control" :title="['流行','新款','精选']"
 @tabClick="tabClick" />
 <goods-list :goods="goods[currentType].list"></goods-list>
 </div>
 <back-top @click.native="backTop"/>
 

</div>
</template>

<script>

import HomeSwiper from './childComps/HomeSwiper'
import RecommendView from './childComps/RecommendView'
import FeatureView from './childComps/FeatureView'

import NavBar from 'components/commo/navbar/NavBar'
import TabControl from 'components/conten/tabControl/TabControl'
import GoodsList from 'components/conten/goods/GoodsList'
import BackTop from 'components/conten/backTop/BackTop'

import {getHomeMultidata,getHomeGoods} from 'network/home'






export default {
    name:"Home",
    components:{
      HomeSwiper,
      RecommendView,
      FeatureView,
       NavBar,
       TabControl,
       GoodsList,
       BackTop
      
    },
  
    data(){
      return{
       banners:[],
       recommends:[],
       goods:{
         'pop':{page:0,list:[]},
         'new':{page:0,list:[]},
         'sell':{page:0,list:[]}
       },
       currentType:'pop'
      }
    },
    created(){
      this.getHomeMultidata(),
      this.getHomeGoods('pop'),
      this.getHomeGoods('new'),
      this.getHomeGoods('sell')
0    },
methods:{


  tabClick(index){
    switch(index){
      case 0:
        this.currentType='pop'
        break
      case 1:
        this.currentType='new'
        break
      case 2:
        this.currentType='sell'
        break
    }
  },

  //网络数据请求
  getHomeMultidata(){
    getHomeMultidata().then(res=>{
      this.banners=res.data.banner.list;
      this.recommends=res.data.recommend.list;
    })
  },
  getHomeGoods(type){
    const page=this.goods[type].page+1;
    getHomeGoods(type,page).then(res=>{
      this.goods[type].list.push(...res.data.list);
      this.goods[type].page+=1;
    })
  }
}
}
</script>

<style scoped>
#home{
  padding-top: 44px;
}
.home-nav{
  background-color: var(--color-tint);
  color: #fff;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 9;
}
.tab-control{
  position: sticky;
  top: 44px;
  z-index: 9;
}
</style>