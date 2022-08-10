<template>
    <div class="content registrationPCPage">
        <Header></Header>
        <Banner></Banner>
        <div class="shuiyin-bg-box">
            <div class="Sign-up">
                <img src="@/assets/img/new1116/cansaiTitle.png" alt="">
                <div class="deadline">（报名信息统计截止日期：12月4日中午12:00）</div>
                <p><button @click="signUpBtnChange()" :disabled="CountdownShow" :class="{CountdowDisabled:CountdownShow}">点击报名注册</button></p>
                <div>
                    <img style="width:38%" src="@/assets/img/new1116/zongjuesaiTitle.png" alt="" srcset="">
                </div>
                <!-- <div class="deadline">（报名截止时间：12月1日 22:00）</div> -->
                <div><button @click="signUpquanguoChange()" :disabled="CountdownShow" :class="{CountdowDisabled:CountdownShow}">{{BtnTitle2}}</button></div>
                <!-- <a href="http://202.105.141.116:5000/"></a> -->
                <!-- <div><img style="width:40%" src="@/assets/img/ossChange/chouqianText.png" alt="" srcset=""></div> -->
                <!-- <div style="margin-top:1vh"><a href="http://cq.imtc.org.cn/"><button>点击开始抽签</button></a></div> -->
                <!-- <div style="margin-top:1vh"><button style="opacity:0.5;cursor: no-drop;">抽签已结束</button></div> -->
            </div>
            <!-- <Organizational></Organizational> -->
        </div>
        <div class="Mask-box" v-if="MaskShow"></div>
        <div class="epidemic-prevention-box" v-if="antiepidemicShow">
            <div class="epidemic-prevention">
                <img src="@/assets/img/new1201/fangyiyaoqiuPc.png?v=12011612" alt="" srcset=""  v-if="matchtypey =='c'">
                <img src="@/assets/img/new1201/fangyiyaoqiuQPC.png?v=12011612" alt="" srcset="" v-if="matchtypey =='b'">
                <p @click="ReadChange()" class="regulations"></p>
                <span class="closeBtn epidemicCloseBtn" @click="closeBtnChange()">X</span>
            </div>
        </div>
        <Footer></Footer>
    </div>
</template>
<script>
import qs from 'qs'
import Header from '@/components/header.vue'
import Footer from '@/components/footer.vue'
import Banner from '@/components/Banner.vue'
import Organizational from '@/components/Organizational.vue'
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
            CountdownShow:false,
            BtnTitle2:'点击报名注册'
        }
    },
    components:{
        Header,
        Banner,
        Organizational,
        Footer
    },
    mounted(){
        if (this._isMobile()) {
            this.h5Show = true;
            this.pcShow = false;
            this.$router.push('/registrationH5')
        } else {
            this.h5Show = false;
            this.pcShow = true;
            this.timeDown2('2021/12/04 12:00');
        };
        document.title = 'IMT-2020(5G)大会'
    },
    methods:{
        _isMobile(){
            let flag = navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i)
            return flag;
        },
        signUpBtnChange(){//5G大会
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
            this.$router.push({path:'/signUpPC',query: {name: 8,id: this.matchtypey}});
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
    .registrationPCPage .Sign-up{
        width: 60%;
        margin: 0 auto;
    }
    .registrationPCPage .Sign-up img{
        width: 100%;
        margin-top: 56px;
    }
    .registrationPCPage .Sign-up button{
        min-width:10vw;
        min-height: 4vh;
        padding: 10px 32px;
        font-size: 18px;
        color: #fff;
        background: #5588f2;
        border: none;
        margin-top: 30px;
        border-radius: 5px;
    }
    .registrationPCPage .Sign-up .CountdowDisabled{
        background: #cfcfcf;
        cursor: no-drop;
    }

    /* 观众报名*/
    .registrationPCPage .audience-registration-box{
        width: 60%;
        height: auto;
        z-index: 1000;
        position: fixed;
        top: 15%;
        left: 19%;
        background: #fff;
    }
    .registrationPCPage .audience-registration{
        width: 1200px;
        height: auto;
        margin: 0 auto;
    }
    .registrationPCPage .audience-registration .list{
        width: 100%;
        height: auto;
        box-sizing: border-box;
        font-size: 16px;
        color: #000;
        text-align: left;
        padding: 0 150px;
        padding-bottom: 114px;
    }
    .registrationPCPage .audience-registration .list .tip{
        font-size: 16px;
        text-align: left;
        line-height: 24px;
        color: #000;
    }
    .registrationPCPage .audience-registration .company-name{
        width: 100%;
        height: auto;
        margin-top: 40px;
    }
    .registrationPCPage .audience-registration  label{
        font-size: 18px;
        color: #000;
        margin-right: 36px;
        width: 80px;
        text-align: left;

    }
    .registrationPCPage .audience-registration .company-name input{
        width: 83%;
        height: 45px;
        border: 1px solid #e0e0e0;
        border-radius: 5px;
        outline-color:#1e7be2;
        padding-left: 15px;
        box-sizing: border-box;
    }
    .registrationPCPage .audience-registration .name{
        margin-top: 24px;
    }
    .registrationPCPage .audience-registration .name input:nth-of-type(1){
        width: 31%;
        height: 45px;
        border: 1px solid #e0e0e0;
        border-radius: 5px;
        outline-color:#1e7be2;
        margin-right:  70px;
        padding-left: 15px;
        box-sizing: border-box;
    }
    .registrationPCPage .audience-registration .name input:nth-of-type(2),.audience-registration .name input:nth-of-type(3){
        width: 20px;
        height: 20px;
        font-size: 18px;
        margin-right: 10px;
        padding-left: 15px;
        box-sizing: border-box;
    }
    .registrationPCPage .audience-registration .name label{
        margin-right: 54px;
    }
    .registrationPCPage .audience-registration .tel input{
        width: 282px;
        height: 46px;
        border: 1px solid #e0e0e0;
        border-radius: 5px;
        outline-color:#1e7be2;
        margin-top: 24px;
        padding-left: 15px;
        box-sizing: border-box;
    }
    .registrationPCPage .audience-registration .tel input:nth-of-type(1){
        margin-right: 70px;
    }
    .registrationPCPage .audience-registration .tel label{
        margin-right: 36px;
    }
    .registrationPCPage .audience-registration .tel label:nth-of-type(2){
         margin-right: 70px;
     }

    .registrationPCPage .audience-registration .mobile{
        margin-top: 20px;
    }
    .registrationPCPage .audience-registration .mobile input{
        width: 282px;
        height: 46px;
        border: 1px solid #e0e0e0;
        border-radius: 5px;
        outline-color:#1e7be2;
         padding-left: 15px;
        box-sizing: border-box;
    }
    .registrationPCPage .audience-registration .mobile label{
        margin-right: 72px;
    }
    .registrationPCPage .audience-registration .mobile input:nth-of-type(1) {
        margin-right: 70px;
        padding-left: 15px;
        box-sizing: border-box;
    }    
    .registrationPCPage .audience-registration .mobile label:nth-of-type(2){
        margin-right:  70px;
    }
    .registrationPCPage .audience-registration .website{
        margin-top: 20px;
    }
    .registrationPCPage .audience-registration .website input{
        width: 282px;
        height: 46px;
        border: 1px solid #e0e0e0;
        border-radius: 5px;
        outline-color:#1e7be2;
        padding-left: 15px;
        box-sizing: border-box;
    }
    .registrationPCPage .audience-registration .website input:nth-of-type(1){
        margin-right: 70px;
        padding-left: 15px;
        box-sizing: border-box;
    }
   .registrationPCPage .audience-registration-box .audience-registration .leaving-message{
       margin-top: 20px;
    }

    .registrationPCPage .audience-registration-box .audience-registration .channel{
        font-size: 16px;
        margin-top: 58px;
        height: 90px;
    }
    .registrationPCPage .audience-registration-box .audience-registration .channel label{
        font-size: 16px;
        float: left;
        width:unset;
        margin: 0 10px;
    }
    .registrationPCPage .audience-registration-box .audience-registration .channel input{
        width: 16px;
        height: 16px;
        float: left;
        margin-top: 5px;
        padding-left: 15px;
        box-sizing: border-box;
    }
    .registrationPCPage .channel-text{
        margin-top: 25px;
    }
    .registrationPCPage .product{
        margin-top: 32px;
    }
    .registrationPCPage .product label{
        float: left;
        font-size: 18px;
    }
    .registrationPCPage .product textarea{
        width: 746px;
        outline-color:#1e7be2;
    }
    .registrationPCPage .submit-btn{
        text-align: center;
        margin-top: 60px;
    }
    .registrationPCPage .submit-btn button{
        width: 126px;
        height: 40px;
        line-height: 40px;
        text-align: center;
        color: #fff;
        font-size: 14px;
        border: none;
        background: #5588f2;
    }

    /* 遮罩层 */
    .registrationPCPage .Mask-box{
        width: 100%;
        height: 100%;
        background: #000;
        opacity: 0.3;
        position: fixed;
        top: 0;
        left: 0;
        z-index: 10000;
    }
    /* 防疫提示 */
    .registrationPCPage .epidemic-prevention-box{
        width: 100%;
        position: fixed;
        left: 0;
        top: 0;
        z-index: 10001;
    }
    .registrationPCPage .epidemic-prevention{
        width: 62%;
        margin: 0 auto;
        text-align: center;
        position: relative;
    }
    .registrationPCPage .epidemic-prevention img{
        width: 93%;
        margin-top: 5%;
    }
    .registrationPCPage .regulations{
        background: transparent;
        width: 60%;
        height: 40px;
        line-height: 40px;
        font-size: 20px;
        color: #fff;
        position: absolute;
        bottom: 2.5vh;
        left: 20%;
        cursor: pointer;
    }
    .registrationPCPage .closeBtn{
        font-size: 20px;
        position: absolute;
        right: 6vh;
        top: 8vh;
        color: #dfdfdf;
        cursor: pointer;
    }
    .shuiyin-bg-box{
        padding-bottom: 60px;
        box-sizing: border-box;
    }
    .registrationPCPage .footer-img-box img{
        margin-top: 0;
    }
    .registrationPCPage .deadline{
        font-size: 1.2rem;
        color: #545555;
        margin-top: 10px;
    }
</style>