<template>
    <div class="content registrationH5Page">
        <H5Header></H5Header>
        <div class="index-bg">
            <img class="title-img" src="@/assets/img/ossChange/zhuceTitleH5.png" alt="">
            <p class="reg-title-text">
                <!-- <img style="width:80%; margin-top:4vh" src="@/assets/img/ossChange/biaogansaiTextH5.png" alt="" srcset=""> -->
                5G大会报名
            </p>
            <div class="deadline">（报名信息统计截止日期：12月4日中午12:00）</div>
            <div><button @click="signUpBtnChange()" :disabled="CountdownShow" :class="{CountdowDisabled:CountdownShow}">{{BtnTitle2}}</button></div>
            <div>
                <!-- <img style="width:80%" src="@/assets/img/ossChange/zhengjiTextH5.png" alt="" srcset=""> -->
                <p class="reg-title-text">第四届“绽放杯”5G应用征集大赛总决赛</p>
            </div>
            <!-- <div class="deadline">（报名截止时间：12月1日 22:00）</div> -->
            <div><button @click="signUpquanguoChange()" :disabled="CountdownShow" :class="{CountdowDisabled:CountdownShow}">{{BtnTitle2}}</button></div>
            <!-- <div><img style="width:80%" src="@/assets/img/ossChange/chouqianText.png" alt="" srcset=""></div> -->
            <!-- <div><a href="http://cq.imtc.org.cn/"><button>点击开始抽签</button></a></div> -->
            <!-- <div><button style="opacity:0.5;cursor: no-drop;">抽签已结束</button></div> -->
            <!-- <Organizational></Organizational> -->
        </div>
        <Footer></Footer>
        <div class="Mask-box" v-if="MaskShow"></div>
        <div class="epidemic-prevention-box" v-if="antiepidemicShow">
            <div class="epidemic-prevention epidemic-preventionH5">
                <img src="@/assets/img/new1201/fangyiyaoqiuH5.png?v=12011549" alt="" srcset="" v-if="matchtypey =='c'">
                <img src="@/assets/img/new1201/fangyiyaoqiuQH5.png?v=12011613" alt="" srcset="" v-if="matchtypey =='b'">
                <span class="closeBtn epidemicCloseBtn" @click="closeBtnChange()">X</span>
                <div class="h5bgClick" @click="ReadChange()"></div>
            </div>
        </div>
    </div>
</template>
<script>
import qs from 'qs'
import H5Header from '@/components/h5Header.vue'
import Header from '@/components/header.vue'
import Footer from '@/components/footer.vue'
import Banner from '@/components/Banner.vue'
// import Organizational from '@/components/Organizational.vue'
export default {
    name:'registration',
    data(){
        return{
            MaskShow:false,
            antiepidemicShow:false,
            signUpShow:false,
            user:{
                enterprise_name:'',
                username:'',
                gender:'男',
                tel:'',
                mobile:'',
                email:'',
                posltion:'',
                message:'',
                matchtypey:'c'
            },
            sex:true,
            h5Show:false,
            pcShow:false,
            CountdownShow:false,
            BtnTitle2:'点击报名注册'
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
            this.timeDown2('2021/12/01 22:00');
        } else {
            this.h5Show = false;
            this.pcShow = true;
            this.$router.push('/registration')
        }
        document.title = 'IMT-2020(5G)大会'
    },
    methods:{
        _isMobile(){
            let flag = navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i)
            return flag;
        },
        signUpBtnChange(){//标杆赛
            this.MaskShow = true;
            this.antiepidemicShow =true;
            this.matchtypey = 'c';
        },
        signUpquanguoChange(){//全国赛
            this.MaskShow = true;
            this.antiepidemicShow =true;
            this.matchtypey = 'b';
        },
         ReadChange(){
            this.$router.push({path:'/signUpH5',query: {name: 8,id: this.matchtypey}});
        },
        closeBtnChange(){
            this.MaskShow=false;
            this.antiepidemicShow=false;
            this.signUpShow=false;
        },
        //倒计时
        timeDown2(endDateStr) {
            //结束时间
            var that=this
             var endDate = new Date(endDateStr);
            //当前时间
            var nowDate = new Date();
            //相差的总秒数
            var totalSeconds = parseInt((endDate - nowDate) / 1000);
            if(totalSeconds<=0){
                that.CountdownShow=true;
                that.BtnTitle2 = "报名已结束";
                return
            }else{
                //延迟一秒执行自己
                  setTimeout(function () {
                      that.timeDown2(endDateStr);
                  }, 1000)
            }
        },
    },
}
</script>
<style scoped>
    .content{
        width: 100%;
        height: 100%;
        position: relative;
    }
    /* 观众报名*/
    .registrationH5Page .audience-registration-box{
        width: 100%;
        height: auto;
        z-index: 1000;
        position: fixed;
        top: 15%;
        left: 19%;
        background: #fff;
        border-radius: 20px;
    }
    .registrationH5Page .audience-registration-boxH5{
        width: 85%;
        top: 15%;
        left: 8%;
        padding-bottom: 10%;
        padding: 0 8vw;
        padding-top: 1.5vh;
        padding-bottom: 3vh;
        box-sizing: border-box;
    }
    .registrationH5Page .audience-registration .list{
        width: 100%;
        height: auto;
        box-sizing: border-box;
        font-size: 16px;
        color: #000;
        text-align: left;
        margin-top: 4vh;
        background: url('../assets/img/ossChange/shuiyinH5-regPage.png') 0 0 no-repeat;
        background-size: contain;
    }
    .registrationH5Page .audience-registration .list div{
        display: flex;
        margin-top: 1.5vh;
        line-height: 4vh;
    }
    .registrationH5Page .audience-registration .list .sex p:nth-of-type(2) input{
        width: 4vw;
        height: 4vh;
        background: #fff;
        float: left;
        border-radius: 100%;
        outline: none;
    }
    .registrationH5Page .audience-registration .list .sex p:nth-of-type(2) label{
        width: 100px;
        float: left;
    }
    .registrationH5Page .audience-registration .list div p{
        flex: 1;
    }
    .registrationH5Page .audience-registration .list div p:nth-of-type(1){
        text-align: left;
        flex: 1;
    }
    .registrationH5Page .audience-registration .list div p label{
        font-size: 60px;
        font-weight: 700;
    }
    .registrationH5Page .audience-registration .list div p input[type = "text"]{
        width: 96%;
        font-size: 60px;
        height: 4vh;
        border: 0.1pt solid #e6e6e6;
        outline: none;
        padding-left: 1vw;
        border-radius: 10px;
        background: transparent;
        box-shadow: none;
    }
    .registrationH5Page .audience-registration .list div p:nth-of-type(2){
        text-align: left;
        flex: 3;
    }
    .registrationH5Page .audience-registration  .TitleH4{
        height: 100px;
        width: 100%;
        font-size: 80px;
        font-weight: 700;
        position: relative;
    }
   .registrationH5Page  .audience-registration  .TitleH4 span{
        display: block;
        width: 18%;
        height: 3px;
        position: absolute;
        top:1.5em;
        left: 45%;
    }
    .registrationH5Page .submit-btn{
        display: block;
        width: 100%;
        text-align: center;
        margin-top: 4vh;
    }
    .registrationH5Page .submit-btn button{
        width: 40%;
        margin: 0 auto;
        line-height: 4vh;
        text-align: center;
        color: #fff;
        font-size: 60px;
        border: none;
        background: #5588f2;
        border-radius: 20px;
    }
    .registrationH5Page .index-bg  .CountdowDisabled{
        background: #cfcfcf;
        cursor: no-drop;
    }
    .registrationH5Page .audience-registration  .submit-btn button{
        margin-top: 1.5vh;
    }
    /* 遮罩层 */
    .registrationH5Page .Mask-box{
        width: 100%;
        height: 100%;
        background: #000;
        opacity: 0.3;
        position: fixed;
        top: 0;
        left: 0;
        z-index: 100;
    }
    /* 防疫提示 */
    .registrationH5Page .epidemic-prevention-box{
        width: 100%;
        position: fixed;
        left: 0;
        top: 4vh;
        z-index: 1000;
    }
    .registrationH5Page .epidemic-prevention{
        width: 62%;
        margin: 0 auto;
        text-align: center;
        position: relative;
    }
    .registrationH5Page .epidemic-prevention-box .epidemic-preventionH5{
        width: 85%;
    }
    .registrationH5Page .h5bgClick{
        position: absolute;
        width: 100%;
        height: 150px;
        bottom: 1vh;
        left: 0;
        z-index: 10000;
    }
    .registrationH5Page .epidemic-prevention img{
        width: 100%;
    }
   .registrationH5Page  .regulations{
        background: #5fbe6d;
        width: 60%;
        height: 40px;
        line-height: 40px;
        font-size: 20px;
        color: #fff;
        position: absolute;
        bottom: 104px;
        left: 20%;
        cursor: pointer;
    }
    .registrationH5Page .epidemic-prevention-box .epidemic-preventionH5 .closeBtn{
        font-size: 6em;
        position: absolute;
        right: 2vw;
        top: 1vh;
        color: #dfdfdf;
        cursor: pointer;
    }
    .registrationH5Page .epidemicCloseBtn{
         top: 90px;
    }
   .registrationH5Page  .index-bg{
        width: 83%;
        margin: 0 auto;
    }
    .registrationH5Page .index-bg .title-img{
        width: 100%;
        margin-top: 12%;
    }
    .registrationH5Page .index-bg button{
        min-width: 24vw;
        padding: 10px 32px;
        background: #5588f2;
        border: none;
        margin: 4vh 0;
        border-radius: 15px;
        color: #fff;
        font-size: 6pt;
    }  
    .reg-title-text{
        font-size: 5em;
        color: #545555;
        text-align: center;
        font-weight: 600;
    }
    .reg-title-text:nth-of-type(1){
        margin-top: 4vh;
    }
    .registrationH5Page  .deadline{
        font-size: 8pt;
        color: #000;
        margin-top: 2vw;
    }
</style>