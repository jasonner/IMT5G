<template>
    <div class="content">
        <img v-if="pcShow" src="@/assets/img/ossChange/danweiPC.png" alt="" srcset="">
        <img class="h5IMG" v-if="h5Show" src="@/assets/img/ossChange/zuzhiBGH5.png" alt="" srcset="">
    </div>
</template>
<script>
export default {
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
    .content{
        width: 100%;
        text-align: center;
    }
    .content img{
        width: 60%;
        margin: 95px 0;
    }
    .content .h5IMG{
        margin-top: 4vh;
    }
    .content .h5IMG{
        width:100%;
    }

</style>