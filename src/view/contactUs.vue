<template>
    <div class="content">
        <div v-if="h5Show">
            <H5Header></H5Header>
            <div class="index-bg">
                <p><img src="@/assets/img/ossChange/callUSH5.png" alt=""></p>
                <p class="list">地址:深圳市福田保税区金华路29号华宝一号楼A座1-4层</p>
                <p class="list">邮箱:zfb@bifnc.cn</p>
                <p class="list">电话:18741271301</p>
                <!-- <p class="list">网址: www.2021imt.com</p> -->
                <!-- <Organizational></Organizational> -->
            </div>
            <Footer></Footer>
        </div>
        <div v-if="pcShow">
            <Header></Header>
            <Banner></Banner>
            <div class="shuiyin-bg-box">
                <!-- 联系我们 -->
                    <div class="call-us">
                        <img src="@/assets/img/new1013/callusPC.png" alt="" srcset="">
                    </div>
                    <div class="call-us-text">
                        <p>地址：深圳市福田保税区金华路29号华宝一号楼A座1-4层  </p>
                        <p>邮箱:zfb@bifnc.cn &nbsp;&nbsp;电话：18741271301</p>
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
import Footer from '@/components/footer.vue'
import Banner from '@/components/Banner.vue'
// import Organizational from '@/components/Organizational.vue'
export default {
    name:'',
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

<style scoped>
    .call-us{
        width: 60%;
        margin: 0 auto;
    }
    .call-us img{
        width: 100%;
        margin-top: 56px;
    }
    .index-bg{
        width: 83%;
        margin: 0 auto;
    }
    .index-bg img{
        width: 100%;
        margin-top: 20%;
    }
    .index-bg .list{
        text-align: left;
        font-size: 60px;
        font-weight: 700;
        color: #545555;
        margin-left: 10%;
        margin-top: 20px;
    }
    .call-us-text{
        margin-top: 4vh;
    }
    .call-us-text p{
        font-size: 18px;
        line-height: 65px;
    }
    .shuiyin-bg-box{
        padding-bottom: 50px;
        box-sizing: border-box;
    }
</style>