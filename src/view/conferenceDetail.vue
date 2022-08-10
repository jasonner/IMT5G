<template>
    <div>
        <div v-if="h5Show">
            <H5Header></H5Header>
            <div class="index-bg">
                <img src="@/assets/img/new1126/dahuigaikuangh5.png" alt="">
                <!-- <Organizational></Organizational> -->
            </div>
            <Footer></Footer>
        </div>
        <div v-if="pcShow">
            <Header></Header>
            <Banner></Banner>
            <div class="shuiyin-bg-box">
                <!-- 大会时间 -->
                <div class="Meeting-time">
                    <img src="@/assets/img/new1126/dahuigaikuangPc.png" alt="" srcset="">
                </div>
                <!-- <Organizational></Organizational> -->
            </div>
            <Footer></Footer>
        </div>
    </div>
</template>
<script>
import H5Header from '@/components/h5Header.vue'
import Header from '@/components/header.vue'
import Banner from '@/components/Banner.vue'
// import Organizational from '@/components/Organizational.vue'
import Footer from '@/components/footer.vue'
export default {
    data(){
        return{
            h5Show:false,
            pcShow:false
        }
    },
    components:{
        H5Header,
        Header,
        Banner,
        // Organizational,
        Footer
    },
    mounted(){
        if (this._isMobile()) {
            this.h5Show = true;
            this.pcShow = false;
        } else {
            this.h5Show = false;
            this.pcShow = true;
        }
        document.title = 'IMT-2020(5G)大会'
    },
    methods:{
        _isMobile(){
            let flag = navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i)
            return flag;
        },
    }
}
</script>
<style  scoped>
.Meeting-time{
    width: 60%;
    margin: 0 auto;
}
.Meeting-time img{
    width: 100%;
    margin-top: 56px;
}
.index-bg{
    width: 83%;
    margin: 0 auto;
    margin-top: 12%;
}
.index-bg img{
    width: 100%;
}
.shuiyin-bg-box{
    padding-bottom: 50px;
    box-sizing: border-box;
}
</style>