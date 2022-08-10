<template>
    <div class="content">
        <header class="header-box" :class="{'active':HeaderlistShow}">
            <div class="header-left">
                <img src="https://oss.sxyweb.com.cn/2021/wj/5GIMT/h5/h5logo.png" alt="" srcset="">
            </div>
            <div class="header-right">
                <img class="showClose" v-if="!HeaderlistShow" @click="HeaderlistChange('1')" src="https://oss.sxyweb.com.cn/2021/wj/5GIMT/h5/header-list.png" alt="" srcset="">
                <img style="width:6%" v-if="HeaderlistShow" @click="HeaderlistChange('2')" src="https://oss.sxyweb.com.cn/2021/wj/5GIMT/h5/header-listX.png" alt="" srcset="">
            </div>
        </header>
        <div class="header-list" v-if="HeaderlistShow">
               <div class="list" @click="goPage('index')">
                    <p>首页</p>
                    <p><img src="@/assets/img/ossChange/arror.png" alt="" srcset=""></p>
               </div>
               <div class="list" @click="goPage('dayActivities')">
                    <p>大会背景</p>
                    <p><img src="@/assets/img/ossChange/arror.png" alt="" srcset=""></p>
               </div>
               <div class="list" @click="goPage('conferenceDetail')">
                   <p>大会概括</p>
                   <p><img src="@/assets/img/ossChange/arror.png" alt="" srcset=""></p>
               </div> 
               <div class="list" @click="goPage('newsCenter')">
                   <p>会议日程</p>
                   <p><img src="@/assets/img/ossChange/arror.png" alt="" srcset=""></p>
               </div> 
               <div class="list" @click="goPage('tradeFair')">
                   <p>直播回看</p>
                   <p><img src="@/assets/img/ossChange/arror.png" alt="" srcset=""></p>
               </div> 
               <div class="list" @click="goPage('registrationH5')">
                   <p>报名注册</p>
                   <p><img src="@/assets/img/ossChange/arror.png" alt="" srcset=""></p>
               </div> 
               <div class="list" @click="goPage('contactUs')">
                   <p>联系我们</p>
                   <p><img src="@/assets/img/ossChange/arror.png" alt="" srcset=""></p>
               </div>  
        </div>
    </div>  
</template>
<script>
export default {
    data(){
        return{
            HeaderlistShow:false
        }
    },
    mounted(){

    },
    methods:{
        HeaderlistChange(flag){
            if(flag =='1'){
                this.HeaderlistShow = true; 
            }else{
                this.HeaderlistShow = false; 
            }
        },
        goPage(path){
            this.HeaderlistShow = false;
            this.$router.push(path);
        },
    },
}
</script>
<style scoped>
    .content{
        width: 100%;
        height: 100%;
    } 
    .header-box{
        padding: 2vh;
        width: 100%;
        vertical-align: middle;
        height: auto;
        display: flex;
        text-align: left;
        z-index: 1000;
        background: #fff;
        box-sizing: border-box;
        box-shadow: 1vh 1vh 1vh 1vh #f6f6f7;
    }
    .header-box div{
        flex: 1;
    }
    .header-left img{
        width: 85%;
        margin-top: 20px;
        margin-left: 1vw;
    }
    .header-right{
        text-align: right;
    }
    .header-right img{
        width: 10%;
        margin-top: 9%;
        margin-right: 1.9vh;
        text-align: right;
    }
    .header-right .showClose{
        width: 10%;
    }
    .header-list{
        width: 100%;
        height: 100%;
        position: fixed;
        background: #fff;
        top:8vh;
        left: 0;
        z-index: 1000;
        padding-top: 1vh;
    }
    .list{
        width: 86%;
        height: 7vh;
        line-height: 7vh;
        margin: 0 auto;
        display: flex;
        border-bottom: 0.1em solid #dfdfdf;
    }
    .list p{
        flex: 1;
    }
    .list p:nth-of-type(1){
       text-align: left;
       font-size: 60px;
       font-weight: 700;
    }
    .list p:nth-of-type(2){
        text-align: right;
    }
    .list p:nth-of-type(2) img{
       width: 1vh;
       margin-top: 1vh;
    }
    .content .active{
        position: fixed;
        top: 0;
        left: 0;
    }
</style>