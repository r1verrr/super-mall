<template>
  <div id="home">
      <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
      <home-swiper :banners="banners"/>
      <recommend-view :recommends= 'recommends'/>
      <FeatureView />
    </div>

</template>

<script>

  import NavBar from 'components/common/navbar/NavBar'

  import HomeSwiper from './childComps/HomeSwiper'
  // import { getHomeMultidata} from "network/home"
  import {getHomeMultidata} from "network/home"
  import RecommendView from './childComps/RecommendView'
  import FeatureView from './childComps/FeatureView'
  



export default {
  name:"home",
  components: {
    NavBar,
    getHomeMultidata,
    HomeSwiper,
    RecommendView,
    FeatureView
  
  },
  data(){
    return {
       banners:[],
       recommends:[]
    }
  },
  // created(){
  //   //1.请求多个数据
  //    getHomeMultidata().then(
  //     res =>{
  //       console.log(res);
  //       this.banners = res.data.banners.list;
  //       this.recommends = res.data.recommends.list;
  //     }
  //   )

  // }
      created() {
        getHomeMultidata().then(res => {
          // this.result = res;
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
        })
      },
}
</script>

<style >
.home-nav{
  background-color: var(--color-tint);
  color: #fff;
}
</style>