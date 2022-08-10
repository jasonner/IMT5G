<template>
    <div class="content footer-img-box">
      <img v-if="pcShow" src="@/assets/img/new1116/organizerPc.png" alt="" srcset="">
      <div class="keep-on-record"  v-if="pcShow">
          <a href="https://beian.miit.gov.cn/#/Integrated/index">
            <img src="@/assets/img/index/police.png" alt="" srcset="">
            粤ICP备20050621号
          </a>
      </div>
      <img class="footer-img-h5" v-if="h5Show" src="@/assets/img/new1116/organizerH5.png" alt="" srcset="">
    </div>
</template>
<script>
export default {
  name: 'Footer',
  props: {
    msg: String
  },
  data(){
    return{
      h5Show:false,
      pcShow:false  
    }
  },
  mounted(){
    if (this._isMobile()) {
        this.h5Show = true;
        this.pcShow = false;
    } else {
        this.h5Show = false;
        this.pcShow = true;
    }
  },
  methods:{
    _isMobile(){
        let flag = navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i)
        return flag;
    },
  }
}
</script>

<style scoped>
   .footer-img-box{
        width: 100%;
        text-align: left;
    }
    .footer-img-box img{
      width: 100%;
      margin-bottom: -3px;
    }
    .footer-img-box .footer-img-h5{
      position: fixed;
      left: 0;
      bottom: 0;
    }

    .keep-on-record{
      position: absolute;
      left: 19%;
      bottom: 15%;
    }
    .keep-on-record a{
      color: #a6c9dd;
      font-size: 14px;
    }
    .keep-on-record a img{
      width: 20px;
    }
</style>