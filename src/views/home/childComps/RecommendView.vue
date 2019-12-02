<template>
  <div class="recommend">
    <div v-for="item in recommends" class="recommends-item">
      <a :href="item.link">
        <img :src="item.image" alt="">
        <div>{{item.title}}</div>
      </a>
    </div>
    </div>
</template>

<script>
export default {
props :{
  recommends :{
    type:Array,
    default(){
      return [ ]
    }
  }
}
}
</script>

<style>
.recommend{
    display: flex;

}
.recommend-item{
  flex:1;
}
</style>