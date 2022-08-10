<template>
    <div class="banner-box">
        <div class="banner">
          <img src="@/assets/img/new1126/bannerPc.png" alt="">
        </div>
    </div>
</template>
<script>
export default {
  name: 'Banner',
  props: {
    msg: String
  },
  data(){
    return{
         
    }
  }
}
</script>

<style scoped>
    /* banner图 */
    .banner-box{
        width: 100%;
        background:#010d57;
    } 
    .banner-box .banner img{
       width: 100%;
       margin-bottom: -3px;
    }
</style>