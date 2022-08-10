<template>
    <div class="content-box tradeFairPage">
        <div v-if="h5Show">
            <H5Header></H5Header>
            <div class="index-bg">
                <p><img src="@/assets/img/ossChange/xianshangzhiboH5.png" alt="" srcset=""></p>
                <div class="title">2021年IMT-2020(5G)大会开幕式暨主论坛</div>
                <div class="fangwenliangH5">访问量:{{num}}次</div>
                <!-- <div class="title">直播回看</div> -->

                <!-- <p><button> <a href="/IMT5Gzhibo/index.html">点击观看直播</a></button></p> -->
                <video src="http://forhoooss.wfbweb.com/2021/1206-5Gmeeting/5g-1206.mp4" poster="@/assets/img/ossChange/kv2.jpg"  controls="controls"  controlsList="nodownload  noplaybackrate" :disablePictureInPicture ="true">
                    您的浏览器不支持 video 标签。
                </video>
            </div>
            <Footer></Footer>
        </div>
        <div v-if="pcShow">
            <Header></Header>
            <Banner></Banner>
            <div class="shuiyin-bg-box pc-box">
                <div class="xianshangzhibo">
                    <img src="@/assets/img/ossChange/xianshangzhiboPC.png" alt="" srcset="">
                    <div class="title">2021年IMT-2020(5G)大会开幕式暨主论坛</div>
                    <div class="fangwenliang">访问量:{{num}}次</div>
                    <!-- <p><button> <a href="/IMT5Gzhibo/index.html">点击观看直播</a></button></p> -->
                    <video ref="video" id="video" src="http://forhoooss.wfbweb.com/2021/1206-5Gmeeting/5g-1206.mp4" poster="@/assets/img/ossChange/kv2.jpg" controls="controls" controlsList="nodownload  noplaybackrate" :disablePictureInPicture ="true">
                        您的浏览器不支持 video 标签。
                    </video>
                </div>
                <!-- <Organizational></Organizational> -->
            </div>    
            <Footer ></Footer>
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
    data(){
        return{
            h5Show:false,
            pcShow:false,
            num:0,
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
        document.title = '2021年IMT-2020(5G)大会开幕式暨主论坛回放';
        this.getLookNum();
    },
    
    methods:{
        _isMobile(){
            let flag = navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i)
            return flag;
        },
        
        getLookNum(){
            let that = this;
            that.axios.get('/home/AddC')
            .then(function (res) {
                console.log(res.data);
                if(res.status == 200 ){
                    if(res.data.msg == 'ok'){
                        that.num = res.data.data.num;
                        console.log(that.num);
                    }
                }else{
                    that.$notify.error({
                        title: '错误',
                        message: '服务异常请稍后重试！'
                    });
                }
            });
        },  
    }
}
</script>
<style  scoped>
    .tradeFairPage{
        width: 100%;
        height: 100%;
    }
    .tradeFairPage .xianshangzhibo{
        width: 60%;
        margin: 0 auto;
        text-align: center;
    }
    .tradeFairPage .xianshangzhibo img{    
        width: 100%;
        margin-top: 56px;
    }
    .tradeFairPage .xianshangzhibo p{
        margin-top: 45px;
     }
    .tradeFairPage .xianshangzhibo button{
        padding: 10px 32px;
        background: #5588f2;
        border: none;
        margin-top: 30px;
        border-radius: 5px;
    }
    .tradeFairPage .xianshangzhibo button a{
        font-size: 18px;
        color: #fff;
     }
    .tradeFairPage .index-bg{
        width: 83%;
        margin: 0 auto;
    }
    .index-bg .title{
        font-size: 4vw;
        font-weight: 700;
        margin-top: 4vw;
    }
     .index-bg video{
        width: 100%;
        margin-top: 4vw;
     }
    .tradeFairPage .index-bg img{
        width: 100%;
        margin-top: 20%;
    }
    .tradeFairPage .index-bg button{
        padding: 10px 32px;
        background: #5588f2;
        border: none;
        margin-top: 60px;
        border-radius: 15px;
    }
    .tradeFairPage .index-bg button a{
        font-size: 60px;
        color: #fff;
     }
     .shuiyin-bg-box{
        padding-bottom: 50px;
        box-sizing: border-box;
     }
     .pc-box .xianshangzhibo video{
         width: 100%;
         margin-top: 30px;
     }
     .pc-box .title{
         font-size: 1vw;
         font-weight: 700;
         color: #545555;
         margin-top: 30px;
     }
     .fangwenliang{
        font-size: 18px;
        margin-top: 20px;
     }
     .fangwenliangH5{
         font-size: 3vw;
         margin-top: 2vw;
     }
</style>