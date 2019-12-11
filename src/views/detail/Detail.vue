<template>
  <div id="detail">
   <DetailNavBar/>
   <DetailSweiper :top-images="topImages"> </DetailSweiper>
  </div>
</template>

<script>
    import DetailNavBar from './childComps/DetailNavBar'
    import DetailSweiper from './childComps/DetailSwiper'

    import {getDetail} from '../../network/detial';

export default {
     name:'Detail',
      components:{
      DetailNavBar,
      DetailSweiper
  },
  data(){
    return {
      iid:null,
      topImages:[]
    }
  },
  created(){
    //1.保存传入的iid
      this.iid = this.$route.params.iid,
      //根据iid请求详细数据
      getDetail(this.iid).then(res=>{
          console.log(res);
          //1.获取顶部轮播数据
          this.topImages = res.result.itemInfo.topImages      
  })
  }
  
  

}
</script>

<style scoped>

</style>