<template>
    <div id="newsCenter">
        <div v-if="h5Show">
            <H5Header></H5Header>
            <div class="index-bg huiyiyichengH5">
                <img src="@/assets/img/new1205/mobileYichengnew.png" alt="" srcset="">
            </div>
            <Footer></Footer>
        </div>
        <div v-if="pcShow">
            <Header></Header>
            <Banner></Banner>
            <div class="shuiyin-bg-box list-content">
                <div class="list-left">
                    <el-row class="tac">
                        <el-col :span="12">
                            <el-menu
                                :default-active="activeIndex"
                                @select="handleSelecte"
                            >
                                <el-submenu index="1">
                                    <template slot="title">
                                        <span style="text-align: center;">会议日程</span>
                                    </template>
                                    <el-menu-item-group>
                                        <el-menu-item index="1-1">一、开幕式暨主论坛议程</el-menu-item>
                                        <el-menu-item index="1-2">二、5G应用产融对接会议程</el-menu-item>
                                        <el-menu-item index="1-3">三、5G+智慧交通技术与产业发展论坛议程</el-menu-item>
                                        <el-menu-item index="1-4">四、5G商业模式创新议程</el-menu-item>
                                        <el-menu-item index="1-5">五、5G应用安全议程</el-menu-item>
                                        <el-menu-item index="1-6">六、5G+医疗健康议程</el-menu-item>
                                        <el-menu-item index="1-7">七、5G+智慧文旅议程</el-menu-item>
                                        <el-menu-item index="1-8">八、5G行业虚拟专网议程</el-menu-item>
                                        <el-menu-item index="1-9">九、5G智慧教育议程</el-menu-item>
                                        <el-menu-item index="1-10">十、5G个人应用议程</el-menu-item>
                                    </el-menu-item-group>
                                </el-submenu>
                            </el-menu>
                        </el-col>
                    </el-row>    
                </div>
                <div class="list-right">
                    <div class="huiyiricheng" v-show="activeIndex=='1-1'">
                        <img src="@/assets/img/new1205/yicheng1new.png" alt="" srcset="">
                    </div>
                    <div class="huiyiricheng" v-show="activeIndex=='1-2'">
                        <img src="@/assets/img/new1205/yicheng2.png" alt="" srcset="">
                    </div>
                    <div class="huiyiricheng" v-show="activeIndex=='1-3'">
                        <img src="@/assets/img/new1205/yicheng3.png" alt="" srcset="">
                    </div>
                    <div class="huiyiricheng" v-show="activeIndex=='1-4'">
                        <img src="@/assets/img/new1205/yicheng4.png" alt="" srcset="">
                    </div>
                    <div class="huiyiricheng" v-show="activeIndex=='1-5'">
                        <img src="@/assets/img/new1205/yicheng5.png" alt="" srcset="">
                    </div>
                    <div class="huiyiricheng" v-show="activeIndex=='1-6'">
                        <img src="@/assets/img/new1205/yicheng6.png" alt="" srcset="">
                    </div>
                    <div class="huiyiricheng" v-show="activeIndex=='1-7'">
                        <img src="@/assets/img/new1205/yicheng7.png" alt="" srcset="">
                    </div>
                     <div class="huiyiricheng" v-show="activeIndex=='1-8'">
                        <img src="@/assets/img/new1205/yicheng8.png" alt="" srcset="">
                    </div>
                     <div class="huiyiricheng" v-show="activeIndex=='1-9'">
                        <img src="@/assets/img/new1205/yicheng9.png" alt="" srcset="">
                    </div>
                     <div class="huiyiricheng" v-show="activeIndex=='1-10'">
                        <img src="@/assets/img/new1205/yicheng10.png" alt="" srcset="">
                    </div>
                </div>
            </div>
            <Footer></Footer>
         </div>
    </div>
</template>
<script>
import H5Header from '@/components/h5Header.vue'
import Header from '@/components/header.vue'
import Banner from '@/components/Banner.vue'
import Organizational from '@/components/Organizational.vue'
import Footer from '@/components/footer.vue'
export default {
    data(){
        return{
            isActive:'1',
            h5Show:false,
            pcShow:false,
            activeIndex:'1-1'
        }
    },
    components:{
        H5Header,
        Header,
        Banner,
        Organizational,
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

        handleSelecte(key, keyPath) {
            this.activeIndex = key;
        },
    }
}
</script>
<style>
    #newsCenter .huiyiricheng{
        width: 100%;
        padding-right: 10vw;
        box-sizing: border-box;
        margin: 0 auto;
    }
    #newsCenter .huiyiricheng img{
        width: 100%;
        margin-top: 56px;
    }
    #newsCenter .index-bg{
        width: 99%;
        margin: 0 auto;
        margin-top: 12%;
    }
    #newsCenter .huiyiyichengH5 img{
        width: 100%;
        object-fit: fill;
    }
    #newsCenter .index-bg img{
        width: 100%;
        margin-bottom: 3vh;
    }
    #newsCenter .shuiyin-bg-box{
        padding-bottom: 300px;
        box-sizing: border-box;
    }
    #newsCenter .list-content{
        display: flex;
    }
    #newsCenter .list-content .list-left{
        width: 25%;
        flex: auto;
        margin-top: 9.5vw;
        text-align: left;
       
    }
    #newsCenter .list-content .list-right{
        width: 75%;
        flex: auto;
    }
    #newsCenter .el-col-12{
        width: 90%;
    }
    #newsCenter .el-submenu__title{
        text-align: center;
        border-top: 1px solid #efefef;
        border-bottom: 1px solid #efefef;
    }
    #newsCenter  .el-menu{
        background: transparent;
    }
    #newsCenter .footer-img-box .footer-img-h5{
        position: relative;
    }
</style>