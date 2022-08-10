<template>
    <div class="content indexPage">
        <div v-if="h5Show">
            <H5Header></H5Header>
            <div class="index-bg">
                 <img src="@/assets/img/new1126/bannerH5.png" alt="">
            </div>
        </div>
        <div v-if="pcShow">
            <Header></Header>
            <!-- banner图 -->
            <Banner></Banner>
            <div class="shuiyin-bg-box">
                <!-- 大会背景 -->
                <div class="General pc">
                    <img src="@/assets/img/new1116/dahuibeijingPC.png" alt="">
                    <p>为了推动5G发展，自2013年以来，在工业和信息化部通信发展司的指导下，中国信息通信研究院等单位连续举办了七届IMT-2020（5G）峰会。当前，以5G为代表的新一代信息通信技术发展迅速，已成为经济社会数字化转型的关键支撑和重要基石。为深入贯彻落实党中央、国务院关于5G发展的决策部署，进一步推动5G产业发展与应用创新，现举办2021年IMT-2020(5G)大会，以引领5G应用发展趋势，拓宽5G产业发展思路。大会将持续打造全球顶级的5G 产业盛会，汇聚产业力量，搭建行业交流合作及产业链协同创新的桥梁，构筑5G赋能百业的展示及推广平台。</p>
                </div>
                <!-- 组织单位 -->
                <!-- <Organizational></Organizational> -->
            </div>
            <!-- 底部 -->
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
    name:'index',
    data(){
        return{
            h5Show:false,
            pcShow:false
        }
    },
    components:{
        H5Header,
        Header,
        Footer,
        Banner,
        // Organizational,
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
        // 了解更多
        lookMore:function(){
            console.log('了解更多');
            console.log(this.axios) 
        },

        //点击查看新闻
        clickLook:function(){
            console.log('点击查看新闻');
        },

        //更多动态
        moreDynamic:function(){
            console.log('更多动态');
        }

    },

}
</script>
<style >
.indexPage .header-box{
    box-shadow: none;
}
.indexPage .content{
    width: 100%;
    height: 100%;
}
.indexPage .General{
    width: 62%;
    margin: 0 auto;
    height: auto;
}
.indexPage .General img{
    width: 100%;
    margin-top: 56px;
    display: block;
}
.indexPage .General p{
    text-align: left;
    font-size: 22px;
    color: #333;
    line-height: 38px;
    margin-top: 60px;
    text-indent:2em
}
.indexPage .index-bg{
    width: 100%;
    min-height: 100%;
}
.indexPage .index-bg img{
    height: 100%;
    width: 100%;
    margin-bottom: -1vh;
}
.indexPage .shuiyin-bg-box{
    padding-bottom: 50px;
    box-sizing: border-box;
}

</style>