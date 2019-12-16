<template>
  <div id="detail">
   <DetailNavBar class="detail-nav"/>
   <scroll class="content" ref="scroll">
       <DetailSweiper :top-images="topImages"> </DetailSweiper>
        <DetailBaseInfo :goods="goods"></DetailBaseInfo>
        <DetailShopInfo :shop="shop"/>
        <DetailGoodsInfo :detailInfo="detailInfo" @imageLoad="imageLoad"/>
        <DetailParamInfo :paramInfo="paramInfo"/>
        <DetialCommentInfo :commentInfo />
   </scroll>
 
  
  </div>
</template>

<script>
    import DetailNavBar from './childComps/DetailNavBar'
    import DetailSweiper from './childComps/DetailSwiper'
    import DetailBaseInfo from './childComps/DetailBaseInfo'
    import DetailShopInfo from './childComps/DetailShopInfo'
    import DetailGoodsInfo from './childComps/DetailGoodsInfo'
    import DetailParamInfo from './childComps/DetailParamInfo'
    import DetialCommentInfo from './childComps/DetialCommentInfo'

    import Scroll from 'components/common/scroll/Scroll'
   

    import {getDetail,Goods,Shop,GoodsParam} from '../../network/detial';

export default {
     name:'Detail',
      components:{
      DetailNavBar,
      DetailSweiper,
      DetailBaseInfo,
      DetailShopInfo,
      DetailGoodsInfo,
      DetailParamInfo,
      DetialCommentInfo,

      Scroll,
      
  },
  data(){
    return {
      iid:null,
      topImages:[],
      goods:{},
      shop:{},
      detailInfo:{},
      paramInfo:{},
      commentInfo:{}

    }
  },
  created(){
    //1.保存传入的iid
      this.iid = this.$route.params.iid,
      //根据iid请求详细数据
      getDetail(this.iid).then(res=>{
        
          console.log(res);
          const data = res.result
          //1.获取顶部轮播数据
          this.topImages = res.result.itemInfo.topImages    
           //获取商品信息
       this.goods = new Goods (data.itemInfo,data.columns,data.shopInfo.services)  
       //创建店铺信息的对象
       this.shop = new Shop (data.shopInfo)
       //保存商品详情数据
       this.detailInfo = data.detailInfo;
       //获取参数信息
       this.paramInfo = new GoodsParam(data.itemParams.info,data.itemParams.rule)
       //取出评论信息

       if(data.rate.cRate !==0){
         this.commentInfo = data.rate.list[0]
       }
  })
  
  },
  methods:{
      imageLoad(){
           this.$refs.scroll.refresh()
      }
          
    

  }
  
  

}
</script>

<style scoped>
#detail {
  position: relative;
  z-index: 9;
  background-color: #ffffff;
  height: 100vh;
}
.detail-nav {
  position: relative;
  z-index: 9;
  background-color: #ffffff
}
.content {
  height: calc(100% - 44px);
}
</style>