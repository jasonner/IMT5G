<template>
    <div class="content dayActivitiesPage">
        <div v-if="h5Show">
            <H5Header></H5Header>
            <div class="index-bg">
                <p><img src="@/assets/img/new1116/dahuibeijingH5.png" alt=""></p>
                <div>
                    为了推动5G发展，自2013年以来，在工业和信息化部通信发展司的指导下，中国信息通信研究院等单位连续举办了七届IMT-2020（5G）峰会。当前，以5G为代表的新一代信息通信技术发展迅速，已成为经济社会数字化转型的关键支撑和重要基石。为深入贯彻落实党中央、国务院关于5G发展的决策部署，进一步推动5G产业发展与应用创新，现举办2021年IMT-2020(5G)大会，以引领5G应用发展趋势，拓宽5G产业发展思路。大会将持续打造全球顶级的5G 产业盛会，汇聚产业力量，搭建行业交流合作及产业链协同创新的桥梁，构筑5G赋能百业的展示及推广平台。
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

<style >
    .dayActivitiesPage .call-us{
        width: 60%;
        margin: 0 auto;
    }
    .dayActivitiesPage .call-us img{
        width: 100%;
        margin-top: 56px;
    }
    .dayActivitiesPage .index-bg{
        width: 83%;
        margin: 0 auto;
        margin-top: 12%;
        margin-bottom: 12%;
    }
   .dayActivitiesPage  .index-bg img{
        width: 100%;
    }
    .dayActivitiesPage .index-bg div{
        font-size: 10pt;
        color: #333;
        line-height: 2;
        text-indent:2em;
        text-align: left;
    }
    .dayActivitiesPage .index-bg .list{
        text-align: left;
        font-size: 80px;
        font-weight: 700;
        color: #545555;
        margin-left: 10%;
        margin-top: 90px;
    }
    .dayActivitiesPage .footer-img-box .footer-img-h5{
        position: relative;
        margin-bottom: -3vh;
    }
</style>