<template>
      <swiper class="detail-swiper">
        <SwiperItem  v-for="item in topImages">
              <img :src="item" alt="">
        </SwiperItem>
      </swiper>
 </template>

<script>
import {Swiper,SwiperItem} from 'components/common/swiper'

export default {
  props:{
    topImages:{
      type:Array,
      default(){
        return []
      }
    }
  },
  components: {
    Swiper,
    SwiperItem
  }

}
</script>

<style scoped>
.detail-swiper{
  height: 300px;
  overflow: hidden;
}
</style>