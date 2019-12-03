<template>
  <div id="home">
      <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
      <home-swiper :banners="banners"/>
      <recommend-view :recommends= 'recommends'/>
      <FeatureView />
      <tab-control class="tab-control" :titles="['流行',' 新款' ,'精选'] " />
    </div>

</template>

<script>

  import NavBar from 'components/common/navbar/NavBar'

  import HomeSwiper from './childComps/HomeSwiper'
  // import { getHomeMultidata} from "network/home"
  import {getHomeMultidata ,getHomeGoods} from "network/home"
  import RecommendView from './childComps/RecommendView'
  import FeatureView from './childComps/FeatureView'
  import TabControl from 'components/content/tabControl/TabControl'



export default {
  name:"home",
  components: {
    NavBar,
    getHomeMultidata,
    HomeSwiper,
    RecommendView,
    FeatureView,
    TabControl
  },
  data(){
    return {
       banners:[],
       recommends:[],
       goods:{
        'pop':{ page:0,list:[]},
        'new':{ page:0,list:[]},
        'sell':{ page:0,list:[]},
       }
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
        this.getHomeMultidata()
        this.getHomeGoods('pop')
        this.getHomeGoods('new')
        this.getHomeGoods('sell')
      },
      methods:{
        getHomeMultidata(){
        getHomeMultidata().then(res => {
          // this.result = res;
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
        })
        },
         getHomeGoods(type){
           const page = this.goods[type].page + 1
           getHomeGoods(type,page).then( res=>{
             this.goods[type].list.push(...res.data.list)
             this.goods[type].page += 1
           })
         }
      }
}
</script>

<style scoped>
.home-nav{
  background-color: var(--color-tint);
  color: #fff;
  
  position: fixed;
  left: 0;
    right: 0;
    top: 0;
    z-index: 9;
}
.tab-control{
  position: sticky;
  top: 44px;
}
</style>


 