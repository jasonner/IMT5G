<template>
    <div class="signUpPCPage">
        <Header></Header>
        <Banner></Banner>
        <!-- 观众报名 -->
        <div class="audience-registration-box shuiyin-bg-box" id="audience">
            <div class="audience-registration">
                <p class="title-img"><img src="@/assets/img/new1116/baomingTitle.png" alt="" srcset=""></p>
                <h4 class="TitleH4">
                    {{baomingTitle}}
                    <span v-if="!isNewa"></span>
                </h4>
                <!-- <div class="deadline" v-if="!isNewa">（报名截止时间：12月1日 22:00）</div> -->
                <div class="deadline" v-if="isNewa">（报名信息统计截止日期：12月4日中午12:00）</div>
                <div class="list">
                    <div class="select-user-type" v-if="!isNewa">
                        <label class="name" for="name" maxlength="18">我是:</label>
                        <el-radio v-model="radio" label="1" @change="radioChange('1')"><span class="user">参赛选手</span></el-radio>
                        <el-radio v-model="radio" label="2" @change="radioChange('2')"><span class="user">评审专家</span></el-radio>
                    </div>
                    <div class="company-name" v-show="allShow ||isNewa">
                        <label class="name" for="name" >姓名:</label>
                        <input type="text" maxlength="8" autocomplete="off" v-model="user.name" id="name" >
                        <label for="mobile">手机:</label>
                        <input type="tel"  maxlength="11" autocomplete="off" v-model="user.mobile" id="mobile" >
                        <label for="email">邮箱:</label>
                        <input type="email" v-model="user.email" id="email" autocomplete="off" >
                    </div>
                    
                    <div class="mobile" v-show="hideSlect && allShow">
                        <label for="tel">项目名称:</label>
                        <input type="text" autocomplete="off"  v-model="user.entryName" id="entryName" >
                        <label for="email" style="margin-right:3.5vw">抽签序号:</label>
                        <input type="email" v-model="user.number" id="email" autocomplete="off" >
                    </div>
                    <div class="mobile" v-show="allShow || isNewa">
                        <label for="mobile">所在单位:</label>
                        <input type="text" autocomplete="off" v-model="user.Company" id="mobile" >
                        <label for="position" style="margin-right:5.5vw">职务:</label>
                        <input type="text" v-model="user.posltion" id="position" >
                    </div>
                    <div class="mobile" v-show="hideSlect && allShow || isNewa">
                        <label for="mobile">所在省份:</label>
                        <input type="text" @click="toAddress()" readonly autocomplete="off" v-model="user.province" id="mobile" >
                        <label for="position" style="margin-right:3.5vw">所在城市:</label>
                        <input type="text" @click="toAddress()" readonly v-model="user.city" id="position" >
                    </div>
                    <div class="Participate-in-sub-forums" v-if="isNewa">
                        <label for="mobile">参与论坛:</label>
                        <el-checkbox v-model="item.checked" v-for="(item,index) in checkedList1" :key="index">{{item.title}}</el-checkbox>
                    </div>
                    <div class="Participate-in-sub-forums2"  v-if="isNewa">
                        <el-checkbox v-model="item.checked" v-for="(item,index) in checkedList2" :key="index">{{item.title}}</el-checkbox>
                    </div>
                </div>
                <div class="Material-Science" v-show="allShow || isNewa">
                    <p>请上传以下证明材料</p>
                    <div class="Material-Science-list">
                        <div class="list-content">
                            <p>本人健康码</p>
                            <el-upload
                                class="upload-demo"
                                drag
                                :limit=1
                                action=""
                                accept="image/*"
                                :http-request="getHealthCode"
                                multiple>
                                <i class="el-icon-upload"></i>
                                <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
                            </el-upload>
                        </div>
                        <div class="list-content">
                            <p>本人行程码</p>
                            <el-upload
                                class="upload-demo"
                                drag
                                :limit=1
                                action=""
                                accept="image/*"
                                :http-request="getTravelCode"
                                multiple>
                                <i class="el-icon-upload"></i>
                                <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
                            </el-upload>
                        </div>
                        <!-- <div class="list-content">
                            <p>本人核酸证明</p>
                            <el-upload
                                class="upload-demo"
                                drag
                                :limit=1
                                action=""
                                accept="image/*"
                                :http-request="getNuclein"
                                multiple>
                                <i class="el-icon-upload"></i>
                                <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
                            </el-upload>
                        </div> -->
                    </div>
                </div>
                 <div class="submit-btn" v-show="allShow ||isNewa">
                    <button type="submit" @click="submissionChange()" :disabled='isSisabled' :class="{CountdowDisabled:isSisabled}">{{BtnTitle2}}</button>
                </div>
            </div>
        </div>
        <v-distpicker type="mobile" @selected='selected' v-show="addInp" class="area1"></v-distpicker>
        <div class="mask" v-show="mask"></div>
    </div>
</template>
<script>
import qs from 'qs'
import OSS from 'ali-oss';
import Header from '@/components/header.vue'
import Banner from '@/components/Banner.vue'
import VDistpicker from 'v-distpicker'
export default {
    name:'registration',
    data(){
        return{
            MaskShow:false,
            antiepidemicShow:false,
            signUpShow:false,
            user:{
                name:'',
                mobile:'',
                email:'',
                entryName:'',
                number:'',
                Company:'',
                posltion:'',
                province:'',
                city:''
            },
            fileurlA:'',
            fileurlB:'',
            fileurlC:'',
            addInp :false,
            mask:false,
            matchtypey:'a',
            baomingTitle:'第四届“绽放杯”5G应用征集大赛标杆赛参会报名',
            radio:'',
            hideSlect:true,
            allShow:false,
            isSisabled:false,
            isNewa:false,//5G报名注册
            checkedList1:[{
                title:'主论坛',
                checked:false
            },{
                title:'5G智慧交通',
                checked:false
            },{
                title:'5G商业模式和创新',
                checked:false
            },{
                title:'5G智慧医疗',
                checked:false
            },{
                title:'5G智慧文旅',
                checked:false
            }],
            checkedList2:[{
                title:'5G行业虚拟专网',
                checked:false
            },{
                title:'5G智慧教育',
                checked:false
            },{
                title:'5G个人应用',
                checked:false
            },{
                title:'5G应用安全',
                checked:false
            }],
            BtnTitle2:'确认提交'
        }
    },
    components:{
        Header,
        Banner,
        VDistpicker
    },
    mounted(){
        var query=this.$route.query;
        if(query.id == 'b'){
           this.baomingTitle  = '第四届“绽放杯”5G应用征集大赛全国赛参会报名';
        }else{
            this.isNewa = true;
            this.baomingTitle  = '5G大会报名'
        }
        this.timeDown2('2021/12/04 12:00');
        this.matchtypey = query.id;
        if (this._isMobile()) {
            this.h5Show = true;
            this.pcShow = false;
            this.$router.push({path:'/signUpH5',query: {name: 8,id:this.matchtypey}});
        } else {
            this.h5Show = false;
            this.pcShow = true;
            
        };
        document.title = 'IMT-2020(5G)大会'
    },
    methods:{
        
        _isMobile(){
            let flag = navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i)
            return flag;
        },

        radioChange(type){
            console.log(type);
            this.allShow = true;
            if(type =='1'){
                this.hideSlect = true;
            }else if(type =='2'){
                this.hideSlect = false;
            }
        },
        //上传健康码
        getHealthCode(file){
            console.log('上传健康码')
            this.Upload(file,'A');
        },
        
        //上传行程码
        getTravelCode(file){
            this.Upload(file,'B');
        },

        //上传核算证明
        getNuclein(file){
           this.Upload(file,'C');
        },

        //自定义上传方法..
        Upload(file,UpType) {
            console.log(file)
            var name = 'IMT'
            let fileName = 'sanmedbio/'+name+'/'+file.file.uid + file.file.name;
            console.log(fileName);
            const _this = this;
            _this.axios.get('https://cac.sanmedbio.com/forhoo/GetOSSSTS?dir=sanmedbio').then((result) => {
                console.log(result.data);
                _this.client =  new OSS({
                region: result.data.region,
                accessKeyId: result.data.data.accessKeyId,
                accessKeySecret: result.data.data.accessKeySecret,
                stsToken: result.data.data.securityToken,
                bucket: result.data.bucketName
                });
                this.getBase64(file.file).then(resBase64 => {
                const base64 = resBase64.split(",").pop();
                const fileType = resBase64.split(";").shift().split(":").pop();
                // base64转blob
                const blob = _this.dataURLtoBlob(base64, fileType);
                console.log(blob);
                // blob转arrayBuffer
                const reader = new FileReader();
                reader.readAsArrayBuffer(blob);
                reader.onload = event => {
                    //arrayBuffer转Buffer
                    const buffer = OSS.Buffer(event.target.result);
                    // 上传
                    _this.client.put(fileName, buffer)
                    .then( result=> {
                        console.log(result);
                    let httpUrl = 'https://merckoss.wfbweb.com/'
                    _this.fileurl = httpUrl+fileName;
                    switch (UpType) {
                        case 'A':
                            _this.fileurlA =  httpUrl+fileName;
                            break;
                        case 'B':
                            _this.fileurlB =  httpUrl+fileName;
                            break;
                        case 'C':
                            _this.fileurlC =  httpUrl+fileName;
                            break;
                        default:
                            break;
                    };
                    console.log(_this.fileurlA);
                    console.log(_this.fileurlB);
                    console.log(_this.fileurlC);
                    })
                    .catch( err=> {
                        console.log("错误", err);
                    });
                };
                })
            })
        },

        //file文件转base64
        getBase64(file) {
            return new Promise((resolve, reject) => {
                let reader = new FileReader();
                let fileResult = "";
                reader.readAsDataURL(file);
        　　　　　//开始转
                reader.onload = function() {
                fileResult = reader.result;
                };
        　　　　　//转 失败
                reader.onerror = function(error) {
                reject(error);
                };
        　　　　　//转 结束 
                reader.onloadend = function() {
                resolve(fileResult);
                };
            });
        },
        
        // base64转blob
        dataURLtoBlob(urlData, fileType) {
            let bytes = window.atob(urlData);
            let n = bytes.length;
            let u8arr = new Uint8Array(n);
            while (n--) {
                u8arr[n] = bytes.charCodeAt(n);
            }
            return new Blob([u8arr], {
                type: fileType
            });
        },

        //提交
        submissionChange(){
            let that = this; 
            if(that.isNewa){
                if(this.user.name == ''){
                    that.$notify.error({
                        title: '错误',
                        message: '请输入姓名'
                    });
                }else if(this.user.mobile ==''){
                    that.$notify.error({
                        title: '错误',
                        message: '请输入手机号'
                    });
                }else if(this.user.email ==''){
                    that.$notify.error({
                        title: '错误',
                        message: '请输入邮箱'
                    });
                }else if(this.user.Company ==''){
                    that.$notify.error({
                        title: '错误',
                        message: '请输入所在单位'
                    });
                }else if(this.user.posltion ==''){
                    that.$notify.error({
                        title: '错误',
                        message: '请输入职务'
                    });
                }else if(this.user.province ==''){
                    that.$notify.error({
                        title: '错误',
                        message: '请输入所在省份'
                    });
                }else if(this.user.city ==''){
                    that.$notify.error({
                        title: '错误',
                        message: '请输入所在城市'
                    });
                }else if(this.fileurlA ==''){
                    that.$notify.error({
                        title: '错误',
                        message: '请上传健康码'
                    });
                }else if(this.fileurlB ==''){
                    that.$notify.error({
                        title: '错误',
                        message: '请上传行程码'
                    });
                }else if(!that.PhoneVerify(this.user.mobile.trim())){
                    that.$notify.error({
                        title: '错误',
                        message: '请输入正确的手机号'
                    });
                }else if(!that.emailVerify(this.user.email.trim())){
                    that.$notify.error({
                        title: '错误',
                        message: '请输入正确的邮箱'
                    });
                }
                else{
                    var canyufenluntan = '';
                    that.checkedList1.forEach(element => {
                        if(element.checked){
                            canyufenluntan+= element.title+','
                        };
                    });
                    that.checkedList2.forEach(element => {
                        if(element.checked){
                            canyufenluntan+= element.title+','
                        };
                    });
                    let promise = {
                        'username':that.user.name,//姓名
                        'mobile':that.user.mobile.trim(),//电话
                        'email':that.user.email.trim(),//邮箱
                        'Company':that.user.Company,//所在单位
                        'post':that.user.posltion,//职务
                        'province':that.user.province,//省份
                        'city':that.user.city,//城市
                        'i1url':that.fileurlA,//健康码
                        'i2url':that.fileurlB,//行程码
                        'i3url':that.fileurlC,//核算证明
                        'matchtype':that.matchtypey,//类型
                        'canyufenluntan':canyufenluntan
                    };
                    that.$confirm('请等待秘书处审核，审核成功将有邮件通知,未收到邮件和收到审核拒绝邮件请勿前往参加。', '提示', {
                        confirmButtonText: '确定',
                        cancelButtonText: '取消',
                        type: 'warning'
                        }).then(() => {
                            that.postMessageChange(promise);
                        }).catch(() => {
                            that.$notify.error({
                                title: '提示',
                                message: '您未报名注册参加'
                            });  
                    });
                }
            }else{
                if(!that.hideSlect){
                    if(this.user.name == ''){
                        that.$notify.error({
                            title: '错误',
                            message: '请输入姓名'
                        });
                    }else if(this.user.mobile ==''){
                        that.$notify.error({
                            title: '错误',
                            message: '请输入手机号'
                        });
                    }else if(this.user.email ==''){
                        that.$notify.error({
                            title: '错误',
                            message: '请输入邮箱'
                        });
                    }else if(this.user.Company ==''){
                        that.$notify.error({
                            title: '错误',
                            message: '请输入所在单位'
                        });
                    }else if(this.user.posltion ==''){
                        that.$notify.error({
                            title: '错误',
                            message: '请输入职务'
                        });
                    }else if(this.fileurlA ==''){
                        that.$notify.error({
                            title: '错误',
                            message: '请上传健康码'
                        });
                    }else if(this.fileurlB ==''){
                        that.$notify.error({
                            title: '错误',
                            message: '请上传行程码'
                        });
                    }else if(!that.PhoneVerify(this.user.mobile)){
                        that.$notify.error({
                            title: '错误',
                            message: '请输入正确的手机号'
                        });
                    }else if(!that.emailVerify(this.user.email.trim())){
                        that.$notify.error({
                            title: '错误',
                            message: '请输入正确的邮箱'
                        });
                    }else{
                        let promise = {
                            'username':that.user.name,//姓名
                            'mobile':that.user.mobile.trim(),//电话
                            'email':that.user.email.trim(),//邮箱
                            'Company':that.user.Company,//所在单位
                            'post':that.user.posltion,//职务
                            'province':that.user.province,//省份
                            'city':that.user.city,//城市
                            'i1url':that.fileurlA,//健康码
                            'i2url':that.fileurlB,//行程码
                            'i3url':that.fileurlC,//核算证明
                            'matchtype':that.matchtypey//类型
                        };
                        that.postMessageChange(promise);
                    }
                }else {
                    if(this.user.name == ''){
                        that.$notify.error({
                            title: '错误',
                            message: '请输入姓名'
                        });
                    }else if(this.user.mobile ==''){
                        that.$notify.error({
                            title: '错误',
                            message: '请输入手机号'
                        });
                    }else if(this.user.email ==''){
                        that.$notify.error({
                            title: '错误',
                            message: '请输入邮箱'
                        });
                    }else if(this.user.entryName==''){
                        that.$notify.error({
                            title: '错误',
                            message: '请输入项目名称'
                        });
                    }else if(this.user.number==''){
                        that.$notify.error({
                            title: '错误',
                            message: '请输入抽签序号'
                        });
                    }else if(this.user.Company ==''){
                        that.$notify.error({
                            title: '错误',
                            message: '请输入所在单位'
                        });
                    }else if(this.user.posltion ==''){
                        that.$notify.error({
                            title: '错误',
                            message: '请输入职务'
                        });
                    }else if(this.user.province ==''){
                        that.$notify.error({
                            title: '错误',
                            message: '请输入所在省份'
                        });
                    }else if(this.user.city ==''){
                        that.$notify.error({
                            title: '错误',
                            message: '请输入所在城市'
                        });
                    }else if(this.fileurlA ==''){
                        that.$notify.error({
                            title: '错误',
                            message: '请上传健康码'
                        });
                    }else if(this.fileurlB ==''){
                        that.$notify.error({
                            title: '错误',
                            message: '请上传行程码'
                        });
                    }else if(!that.PhoneVerify(this.user.mobile.trim())){
                        that.$notify.error({
                            title: '错误',
                            message: '请输入正确的手机号'
                        });
                    }else if(!that.emailVerify(this.user.email.trim())){
                        that.$notify.error({
                            title: '错误',
                            message: '请输入正确的邮箱'
                        });
                    }else{
                        let promise = {
                            'username':that.user.name,//姓名
                            'mobile':that.user.mobile.trim(),//电话
                            'email':that.user.email.trim(),//邮箱
                            'project_name':that.user.entryName,//项目名称
                            'project_id':that.user.number,//项目分组及编号
                            'Company':that.user.Company,//所在单位
                            'post':that.user.posltion,//职务
                            'province':that.user.province,//省份
                            'city':that.user.city,//城市
                            'i1url':that.fileurlA,//健康码
                            'i2url':that.fileurlB,//行程码
                            'i3url':that.fileurlC,//核算证明
                            'matchtype':that.matchtypey//类型
                        };
                        that.postMessageChange(promise);
                    } 
                }
            }

        },
        postMessageChange(promise){
            let that = this;
            that.isSisabled = true;
            setTimeout(() => {
                that.isSisabled = false;
            }, 30000);
            that.axios.post('/home/IMT0927_AddUserInfo', qs.stringify(promise))
            .then(function (res) {
                console.log(res);
                if(res.status == 200 ){
                    if(res.data && res.data.success && res.data.success != 'false'){
                       that.$notify({
                            title: '成功',
                            message: '恭喜你，报名成功',
                            type: 'success'
                        });
                        that.MaskShow = false;
                        that.antiepidemicShow = false;
                        that.signUpShow = false;
                        that.$router.push('/registration')
                    }else{
                        that.$notify.error({
                            title: '错误',
                            message: res.data.msg
                        });
                    }
                }else{
                    that.$notify.error({
                        title: '错误',
                        message: '服务异常请稍后重试！'
                    });
                }
            });
        },
        //手机号检验
        PhoneVerify(str){
            var reg = /^[1][3,4,5,6,7,8,9][0-9]{9}$/
            if (reg.test(str)) {
                return  true;
            }else{
                return false;
            }
        },

        //邮箱验证
        emailVerify(email){
            //var reg = /^([a-zA-Z0-9]+[_|\_|\.]?)*[a-zA-Z0-9]+@([a-zA-Z0-9]+[_|\_|\.]?)*[a-zA-Z0-9]+\.[a-zA-Z]{2,3}$/;
            var reg = /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/
            if(reg.test(email)){
                return true
            }else{
                return false
            }
        },
        toAddress(){
            this.mask = true;
            this.addInp = true;
        },
        // 省市区三级联动
        selected(data){
            this.mask =false;
            this.addInp = false;
            this.user.province = data.province.value;
            this.user.city = data.city.value +' ' + data.area.value;
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
                that.isSisabled = true;
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
    .signUpPCPage .Sign-up{
        width: 60%;
        margin: 0 auto;
    }
    .signUpPCPage .Sign-up img{
        width: 100%;
        margin-top: 56px;
    }
    .signUpPCPage .Sign-up button{
        padding: 10px 32px;
        font-size: 18px;
        color: #fff;
        background: #5588f2;
        border: none;
        margin-top: 30px;
        border-radius: 5px;
    }
    /* 观众报名*/
    .signUpPCPage .audience-registration-box{
        width: 100%;
        margin: 0 auto;
        height: auto;
        z-index: 1000;
        background: url('../assets/img/ossChange/bg1PC-signUpPC.png') 50% 80% no-repeat;
        background-size:cover;
        padding-bottom: 164px;
        box-sizing: border-box;
    }
    .signUpPCPage .audience-registration{
        width: 80%;
        height: auto;
        margin: 0 auto;
        margin-top: 56px;
    }
    .signUpPCPage .audience-registration .list{
        width: 100%;
        height: auto;
        box-sizing: border-box;
        font-size: 16px;
        color: #000;
        text-align: left;
        padding-left: 15.2vw;
        box-sizing: border-box;
    }
    .signUpPCPage .audience-registration .list .tip{
        font-size: 16px;
        text-align: left;
        line-height: 24px;
        color: #000;
    }
    .signUpPCPage .audience-registration .company-name{
        width: 100%;
        height: auto;
        margin-top: 40px;
    }
    .signUpPCPage .audience-registration .name{
        margin-right: 3.8vw;
    }
    .signUpPCPage .audience-registration  label{
        font-size: 18px;
        color: #000;
        margin-right: 36px;
        text-align: left;
        line-height: 2;
    }
    .signUpPCPage .audience-registration .company-name input{
        width: 17.2%;
        height: 45px;
        border: 1px solid #e0e0e0;
        border-radius: 5px;
        outline-color:#1e7be2;
        padding-left: 15px;
        box-sizing: border-box;
        margin-right: 30px;
    }
    .signUpPCPage .audience-registration .name{
        margin-top: 24px;
    }
    .signUpPCPage .audience-registration .name input:nth-of-type(1){
        width: 31%;
        height: 45px;
        border: 1px solid #e0e0e0;
        border-radius: 5px;
        outline-color:#1e7be2;
        margin-right:  70px;
        padding-left: 15px;
        box-sizing: border-box;
    }
    .signUpPCPage .audience-registration .name input:nth-of-type(2),.audience-registration .name input:nth-of-type(3){
        width: 20px;
        height: 20px;
        font-size: 18px;
        padding-left: 15px;
        box-sizing: border-box;
    }
    .signUpPCPage .audience-registration .tel input{
        width: 282px;
        height: 46px;
        border: 1px solid #e0e0e0;
        border-radius: 5px;
        outline-color:#1e7be2;
        margin-top: 24px;
        padding-left: 15px;
        box-sizing: border-box;
    }
    .signUpPCPage .audience-registration .tel input:nth-of-type(1){
        margin-right: 70px;
    }
    .audience-registration .mobile{
        margin-top: 20px;
    }
    .signUpPCPage .audience-registration .mobile input{
        width: 320px;
        height: 46px;
        border: 1px solid #e0e0e0;
        border-radius: 5px;
        outline-color:#1e7be2;
        padding-left: 15px;
        box-sizing: border-box;
    }
    .signUpPCPage .audience-registration .mobile input:nth-of-type(1) {
        margin-right: 70px;
        padding-left: 15px;
        box-sizing: border-box;
    }    
    .signUpPCPage .audience-registration .website{
        margin-top: 20px;
    }
    .signUpPCPage .audience-registration .website input{
        width: 282px;
        height: 46px;
        border: 1px solid #e0e0e0;
        border-radius: 5px;
        outline-color:#1e7be2;
        padding-left: 15px;
        box-sizing: border-box;
    }
    .signUpPCPage .audience-registration .website input:nth-of-type(1){
        margin-right: 70px;
        padding-left: 15px;
        box-sizing: border-box;
    }
   .signUpPCPage .audience-registration-box .audience-registration .leaving-message{
       margin-top: 20px;
    }

    .signUpPCPage .audience-registration-box .audience-registration .channel{
        font-size: 16px;
        margin-top: 58px;
        height: 90px;
    }
    .signUpPCPage .audience-registration-box .audience-registration .channel label{
        font-size: 16px;
        float: left;
        width:unset;
        margin: 0 10px;
    }
    .signUpPCPage .audience-registration-box .audience-registration .channel input{
        width: 16px;
        height: 16px;
        float: left;
        margin-top: 5px;
        padding-left: 15px;
        box-sizing: border-box;
    }
    .signUpPCPage .channel-text{
        margin-top: 25px;
    }
    .signUpPCPage .product{
        margin-top: 32px;
    }
    .signUpPCPage .product label{
        float: left;
        font-size: 18px;
    }
    .signUpPCPage .product textarea{
        width: 746px;
        outline-color:#1e7be2;
    }
    .signUpPCPage .submit-btn{
        text-align: center;
        margin-top: 160px;
    }
    .signUpPCPage .submit-btn button{
        width: 126px;
        height: 40px;
        line-height: 40px;
        text-align: center;
        color: #fff;
        font-size: 14px;
        border: none;
        background: #5588f2;
    }
     .signUpPCPage .submit-btn .CountdowDisabled{
        background: #cfcfcf;
        cursor: no-drop;
     }

    /* 遮罩层 */
    .signUpPCPage .Mask-box{
        width: 100%;
        height: 100%;
        background: #000;
        opacity: 0.3;
        position: fixed;
        top: 0;
        left: 0;
        z-index: 100;
    }
    .signUpPCPage .audience-registration .TitleH4{
        font-size: 28px;
        position: relative;
        color: #545555;
        margin-bottom: 20px;
    }
    .signUpPCPage .audience-registration .TitleH4 span{
        position: absolute;
        left: 39.5vw;
        top: 40px;
        width: 70px;
        height: 3px;
        background: #5588f2;
        border-radius: 50px;
    }
    /* 防疫提示 */
    .signUpPCPage .epidemic-prevention-box{
        width: 100%;
        position: fixed;
        left: 0;
        top: 0;
        z-index: 1000;
    }
    .signUpPCPage .epidemic-prevention{
        width: 62%;
        margin: 0 auto;
        text-align: center;
        position: relative;
    }
    .signUpPCPage .epidemic-prevention img{
        width: 100%;
        margin-top: 5%;
    }
    .signUpPCPage .regulations{
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
    .signUpPCPage .closeBtn{
        font-size: 20px;
        position: absolute;
        right: 20px;
        top: 10px;
        color: #dfdfdf;
        cursor: pointer;
    }
    .signUpPCPage .epidemicCloseBtn{
         top: 90px;
    }
    /* 上传材料 */
    .signUpPCPage .Material-Science{
        width: 68%;
        margin: 0 auto;
        margin-top: 75px;
    }
    .signUpPCPage .Material-Science p{
        font-size: 22px;
    }
    .signUpPCPage .Material-Science .Material-Science-list{
        width: 100%;
        display: flex;
        margin-top: 60px;
    }
    .Material-Science .Material-Science-list .list-content{
        height: 220px;
        width: 320px;
        margin: 0 10px;
        border: 1px dashed #bbbbbb;
        border-radius: 10px;
        flex: 1;
    }
    .signUpPCPage .Material-Science .Material-Science-list .list-content p{
        font-size: 18px;
        margin-top: 40px;
     }
    .signUpPCPage .audience-registration-box .el-upload-dragger{
        background: transparent;
        border: none;
    }
    .signUpPCPage .mask{
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: #000;
        opacity: 0.3;
        z-index: 10;
    }
    .signUpPCPage .area1{
        background: #fff;
        z-index: 1000;
        width: 100%;
        height: 28%;
        position:fixed;
        left: 0;
        bottom: 0;
        overflow-y: scroll;
    }
    .signUpPCPage .distpicker-address-wrapper .address-container ul li{
        font-size: 12pt;
        line-height: 5vh;
        color: #000;
    }
    .signUpPCPage .distpicker-address-wrapper .address-header ul li{
        font-size: 12pt;
        line-height: 5vh;
        color: #000;
    }
    .signUpPCPage .select-user-type{
        margin-top: 6vh;
    }
    .signUpPCPage .select-user-type .user{
        font-size: 18px;
        color: #000;
    }
    .signUpPCPage .title-img img{
        width: 62%;
        margin-bottom: 55px;
    }
    .signUpPCPage .Participate-in-sub-forums{
        width: 100%;
        margin-top: 40px;
    }
    .signUpPCPage  .Participate-in-sub-forums2{
        width: 100%;
        margin-top: 10px;
        padding-left: 6.8vw;
    }
    .signUpPCPage .el-checkbox__label{
        font-size: 1.2rem;
    }   
    .signUpPCPage .deadline{
        font-size: 1.2rem;
        color: #000;
    }
</style>