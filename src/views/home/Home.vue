<template>
  <div id="home">
      <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <tab-control :titles="['流行',' 新款' ,'精选'] " 
                    @tabClick='tabClick'
                    ref="tabControl1"
                    class="tab-control"
                    v-show="isTabFixed"/>
    <scroll class="content" 
            ref="scroll"
            :probe-type="3"
            @scroll="contentScroll" 
            :pull-up-load="true" 
            @pullingUp="loadMore">
      <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"/>
      <recommend-view :recommends= 'recommends'/>
      <FeatureView />
      <tab-control :titles="['流行',' 新款' ,'精选'] " 
                    @tabClick='tabClick'
                    ref="tabControl2"/>
      <goods-list :goods="showGoods"/>
    </scroll>
   <back-top @click.native="backClick" v-show="isShowBackTop"/>
    </div>

</template>

<script>

  import NavBar from 'components/common/navbar/NavBar'

  import HomeSwiper from './childComps/HomeSwiper'
  // import { getHomeMultidata} from "network/home"n
  import {getHomeMultidata ,getHomeGoods} from "network/home"
  import RecommendView from './childComps/RecommendView'
  import FeatureView from './childComps/FeatureView'
  import TabControl from 'components/content/tabControl/TabControl'
  import GoodsList from 'components/content/goods/GoodsList'
  import Scroll from  'components/common/scroll/Scroll'
  import BackTop from 'components/content/backTop/BackTop'
  import {debounce} from "common/utils"


export default {
  name:"home",
  components: {
    NavBar,
    getHomeMultidata,
    HomeSwiper,
    RecommendView,
    FeatureView,
    TabControl,
    GoodsList,
    Scroll,
    BackTop
  },
  data(){
    return {
       banners:[],
       recommends:[],
       goods:{
        'pop':{ page:0,list:[]},
        'new':{ page:0,list:[]},
        'sell':{ page:0,list:[]},
       },
       currentType:'pop',
       isShowBackTop:false,
       tabOffsetTop: 0,
       isTabFixed :false,
       saveY:0,
    }
  },
    computed:{
      showGoods(){
        return this.goods[this.currentType].list
      }
    },
      
      activated(){
            this.$refs.scroll.scrollTo(0,this.saveY,0)
            this.$refs.scroll.refresh()
            console.log( this.$refs.scroll.scrollTo(0, this.saveY, 0))
      },
      deactivated(){
            this.saveY = this.$refs.scroll.getScrollY()
            console.log(this.saveY)
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
        //.请求多个数据
        this.getHomeMultidata()

        //2.请求商品数据
        this.getHomeGoods('pop')
        this.getHomeGoods('new')
        this.getHomeGoods('sell')
      //3.
       
      },

      // mounted(){
      //   this.scroll = new BScroll(document.querySelector('.wrapper'))
      // },
        mounted(){
             //1.监听item中图片加载完成
             const refresh = debounce(this.$refs.scroll.refresh,200)
            this.$bus.$on('itemImageLoad',()=>{
              refresh()
           })
            //2.获取tabControl的offsetTop
            //所有组件都有一个属性$el：用于获取组件中的元素
                 
        },
      methods:{
          // 事件监听
          
          tabClick(index){
              switch(index){
                case 0:
                  this.currentType = 'pop'
                 break
                 case 1: 
                 this.currentType = 'new'
                 break 
                 case 2:
                this.currentType = 'sell'
                break
              }
              this.$refs.tabControl1.currentIndex = index;
              this.$refs.tabControl2.currentIndex = index;
          },
          backClick(){
            this.$refs.scroll.scrollTo(0,0)
          },
           contentScroll(position){
             //1.判断BackTop是否显示
             this.isShowBackTop = (-position.y) > 1000
             //2.决定tabControl是否吸顶（position：fixed）
              this.isTabFixed = (-position.y) > this.tabOffsetTop
           },
           loadMore(){
            this.getHomeGoods(this.currentType)
           },
           swiperImageLoad(){
              this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop
           },
        // 网络请求
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

             this.$refs.scroll.finishPullUp()
           })
         }
      }
}
</script>

<style scoped>

 #home {
    
    height: 100vh;
    position: relative;
  }

.home-nav{
  background-color: var(--color-tint);
  color: #fff;

/* width:100%;
    display:-webkit-box;
    display:flex;
    overflow-y:scroll;
    white-space:nowrap; */


  
  /* position:fixed; */
  /* left: 0;
    right: 0;
    top: 0;
    z-index: 9; */
}

.content {
    overflow: hidden;

    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }

.tab-control{
  position: relative;
  z-index: 9;
  background-color: #fff
}


</style>


 