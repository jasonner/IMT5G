<template>
    <div class="content signUpH5">
        <!-- <H5Header></H5Header> -->
        <header class="header-box">
            <div class="header-left">
                <img src="https://oss.sxyweb.com.cn/2021/wj/5GIMT/h5/h5logo.png" alt="" srcset="">
            </div>
        </header>
        <!-- 观众报名 -->
        <div class="audience-registration-boxH5" id="audience" >
            <div class="audience-registration">
                <div class="TitleH4">
                    <p>5G大会报名  VIP通道</p>
                    <p style="font-size:12px;margin-top:10px">(非邀请嘉宾，请勿报名)</p>
                </div>
                <div class="list">
                    <div class="user-select" v-if="!isNewa">
                        <p><label>我是:</label></p>
                        <p><input type="radio"  @change="radioChange('1')" name="1" id="radio1"><label for="radio1">参赛选手</label><input @change="radioChange('2')" type="radio" name="1" id="radio2"><label for="radio2">评审专家</label></p>
                    </div>
                    <div v-show="allShow || isNewa">
                        <p><label for="companyName">姓名:</label></p>
                        <p><input type="text" autocomplete="off" v-model="user.name" id="companyName"></p>
                    </div>
                    <div v-show="allShow  || isNewa">
                        <p><label for="mobile">手机:</label></p>
                        <p><input type="text" maxlength="11" autocomplete="off" v-model="user.mobile" id="mobile"></p>
                    </div>
                    <div v-show="allShow  || isNewa">
                        <p><label for="email">邮箱:</label></p>
                        <p><input type="text" v-model="user.email" id="email" autocomplete="off"></p>
                    </div>
                    <div v-show="hideSlect && allShow">
                        <p><label for="entryName">项目名称:</label></p>
                        <p><input type="text" autocomplete="off" v-model="user.entryName" id="entryName"></p>
                    </div>
                    <div v-show="hideSlect && allShow">
                        <p style="flex:2"><label for="number">项目分组及编号:</label></p>
                        <p><input type="text" autocomplete="off" v-model="user.number" id="number" ></p>
                    </div>
                    <div v-show="allShow  || isNewa">
                        <p><label for="Company">所在单位:</label></p>
                        <p><input type="text" autocomplete="off" v-model="user.Company" id="Company"></p>
                    </div>
                    <div v-show="allShow || isNewa"> 
                        <p><label for="position">职务:</label></p>
                        <p><input type="text" v-model="user.posltion" id="position"></p>
                    </div>
                    <div v-show="hideSlect && allShow || isNewa">
                        <p><label for="province">所在省份:</label></p>
                        <p><input type="text" readonly  autocomplete="off" @click="toAddress()"  v-model="user.province" id="province"></p>
                    </div>
                    <div v-show="hideSlect && allShow || isNewa">
                        <p><label for="city">所在城市:</label></p>
                        <p><input type="text" readonly  autocomplete="off"  @click="toAddress()"  v-model="user.city" id="city"></p>
                    </div>
                   <div class="Participate-in-sub-forums" v-if="isNewa">
                        <p><label for="mobile">参与分论坛:</label></p>
                        <p><el-checkbox v-model="item.checked" v-for="(item,index) in checkedList1" :key="index">{{item.title}}</el-checkbox></p>
                    </div>
                    <div class="Participate-in-sub-forums2"  v-if="isNewa">
                        <p><el-checkbox v-model="item.checked" v-for="(item,index) in checkedList2" :key="index">{{item.title}}</el-checkbox></p>
                    </div>
                </div>
                <div class="Upload-file-tip" v-show="allShow || isNewa">
                    <p>请上传以下证明材料</p>
                </div>
                <div class="Upload-file" v-show="allShow || isNewa">
                    <div class="Upload-file-list">
                        <p>本人健康码</p>
                        <div @click="getHealthCode()" v-if="fileurlA ==''">选择文件</div>
                        <input
                            type="file"
                            accept="image/*"
                            style="display:none"
                            @change="changeImg('A')"
                            name="upload_file"
                            ref="inputA"
                            
                        />
                        <div class="uploads" v-if="fileurlA!=''" @click="getHealthCode()">
                            <img :src="fileurlA" alt />
                        </div>
                    </div>
                    <div class="Upload-file-list">
                        <p>本人行程码</p>
                        <div @click="getTravelCode()" v-if="fileurlB ==''">选择文件</div>
                        <input
                            type="file"
                            accept="image/*"
                            style="display:none"
                            @change="changeImg('B')"
                            name="upload_file"
                            ref="inputB"
                            
                        />
                        <div class="uploads" v-if="fileurlB!=''" @click="getHealthCode()">
                            <img :src="fileurlB" alt />
                        </div>
                    </div>
                    <!-- <div class="Upload-file-list">
                        <p>本人核酸证明</p>
                        <div @click="getNuclein()" v-if="fileurlC==''">选择文件</div>
                        <input
                            type="file"
                            accept="image/*"
                            style="display:none"
                            @change="changeImg('C')"
                            name="upload_file"
                            ref="inputC"
                            
                        />
                        <div class="uploads" v-if="fileurlC!=''" @click="getHealthCode()">
                            <img :src="fileurlC" alt />
                        </div>
                    </div> -->
                </div>
                <div class="submit-btn" v-show="allShow || isNewa">
                    <button type="submit" @click="submissionChange()" :disabled='isSisabled' style="background:#bfbfbf">报名已结束</button>
                </div>
            </div>
        </div>
        <v-distpicker type="mobile" @selected='selected' v-show="addInp" class="area1"></v-distpicker>
        <div class="mask" v-show="mask"></div>
    </div>
</template>
<script>
import OSS from 'ali-oss';
import qs from 'qs'
import H5Header from '@/components/h5Header.vue'
import Header from '@/components/header.vue'
import Footer from '@/components/footer.vue'
import Banner from '@/components/Banner.vue'
import Organizational from '@/components/Organizational.vue'
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
            h5Show:false,
            pcShow:false,
            fileurlA:'',
            fileurlB:'',
            fileurlC:'',
            city:'请选择',
            addInp :false,
            mask:false,
            baomingTitle:'',
            hideSlect:true,
            allShow:false,
            isSisabled:true,
            matchtypey:'d',//vip报名
            isNewa:false,
            checkedList1:[{
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
            },]
        }
    },
    components:{
        H5Header,
        Header,
        Banner,
        Organizational,
        Footer,
        VDistpicker
    },
    mounted(){
        // var query=this.$route.query;
        this.baomingTitle  = '5G大会报名'
        this.isNewa = true;
        if (this._isMobile()) {
            this.h5Show = true;
            this.pcShow = false;
        } else {
            this.h5Show = false;
            this.pcShow = true;
            this.$router.push({path:'/vipSignUp',query: {name: 8,id:this.matchtypey}});
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
        getHealthCode(){
            console.log('上传健康码')
            this.$refs.inputA.dispatchEvent(new MouseEvent('click'));
        },
        
        //上传行程码
        getTravelCode(){
            this.$refs.inputB.dispatchEvent(new MouseEvent('click'));
        },

        //上传核算证明
        getNuclein(){
            this.$refs.inputC.dispatchEvent(new MouseEvent('click'));
        },
        
        //上传图片
        changeImg (type) {
            switch (type) {
                case 'A':
                    var files = this.$refs.inputA.files;
                    this.Upload(files[0],type);
                    break;
                case 'B':
                    var files = this.$refs.inputB.files;
                    this.Upload(files[0],type);
                    break;
                 case 'C':
                    var files = this.$refs.inputC.files;
                    this.Upload(files[0],type);
                    break;
                default:
                    break;
            }
        },

        //自定义上传方法..
        Upload(file,UpType) {
            console.log(file)
            var name = 'IMT'
            let fileName = 'sanmedbio/'+name+'/'+file.lastModified + file.name;
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
                this.getBase64(file).then(resBase64 => {
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
            console.log(this.user);
            if(this.user.name == ''){
                alert('请输入姓名');
            }else if(this.user.mobile ==''){
                alert('请输入手机号');
            }else if(this.user.email ==''){
                alert('请输入邮箱');
            }else if(this.user.Company ==''){
                alert('请输入所在单位');
            }else if(this.user.posltion ==''){
                alert('请输入职务');
            }else if(this.user.province ==''){
                alert('请输入所在省份');
            }else if(this.user.city ==''){
                alert('请输入所在城市');
            }else if(this.fileurlA ==''){
                    alert('请上传健康码');
            }else if(this.fileurlB ==''){
                alert('请上传行程码');
            }else if(!that.PhoneVerify(this.user.mobile.trim())){
                alert('请输入正确的手机号');
            }else if(!that.emailVerify(this.user.email.trim())){
                alert('请输入正确的邮箱');
            }else{
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
                    'matchtype':that.matchtypey,
                    'canyufenluntan':canyufenluntan
                };
                that.postMessageChange(promise);
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
                        if(that.isNewa){
                            alert('请等待工作人员审核，审核成功将有邮件通知。未收到邮件和收到审核拒绝邮件请勿前往参加。');
                        }else{
                            alert('恭喜你，报名成功');
                        };
                        that.MaskShow = false;
                        that.antiepidemicShow = false;
                        that.signUpShow = false;
                        //that.$router.push('/registrationH5')
                    }else{
                        alert(res.data.msg);
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
            //var reg = /^((13\d)|(14[5,7,9])|(15[0-3,5-9])|(166)|(17[0,1,3,5,7,8])|(18[0-9])|(19[8,9]))\d{8}/;
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
            this.city = data.province.value + ' ' + data.city.value +' ' + data.area.value
            console.log(city);
            this.user.province = data.province.value;
            this.user.city = data.city.value +' ' + data.area.value;
        },
    },
}
</script>

<style >
    .signUpH5 .header-box{
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
    .header-left img{
        width: 85%;
        margin-top: 20px;
        margin-left: 1vw;
    }
    .header-box div{
        flex: 1;
    }
    .content{
        width: 100%;
        height: 100%;
        position: relative;
    }
    /* 观众报名*/
    .signUpH5 .audience-registration-boxH5 .audience-registration{
        width: 100%;
        margin-top: 3.5vh;
    } 
    .signUpH5 .audience-registration-boxH5{
        width: 100%;
        padding-bottom: 10%;
        box-sizing: border-box;
    }
    .signUpH5 .audience-registration .list{
        width: 100%;
        height: auto;
        box-sizing: border-box;
        font-size: 16px;
        color: #000;
        text-align: left;
        margin-top: 2vh;
        padding: 0 8vw;
    }
    .signUpH5 .audience-registration .list div{
        display: flex;
        margin-top: 1.5vh;
        line-height: 4vh;
    }
    .signUpH5 .audience-registration .list .sex p:nth-of-type(2) input{
        width: 4vw;
        height: 4vh;
        background: #fff;
        float: left;
        border-radius: 100%;
        outline: none;
    }
    .signUpH5 .audience-registration .list .sex p:nth-of-type(2) label{
        width: 100px;
        float: left;
    }
    .signUpH5 .audience-registration .list div p{
        flex: 1;
    }
    .signUpH5 .audience-registration .list div p:nth-of-type(1){
        text-align: left;
        flex: 1;
    }
    .signUpH5 .audience-registration .list div p label{
        font-size: 60px;
        font-weight: 700;
    }
    .signUpH5 .audience-registration .list div p input[type = "text"]{
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
    .signUpH5 .audience-registration .list div p:nth-of-type(2){
        text-align: left;
        flex: 3;
    }
    .signUpH5 .audience-registration  .TitleH4{
        height: 100px;
        width: 100%;
        font-size: 80px;
        font-weight: 700;
        position: relative;
    }
    .signUpH5 .audience-registration  .TitleH4 span{
        display: block;
        width: 18%;
        height: 3px;
        position: absolute;
        top:1.5em;
        left: 45%;
    }
    .signUpH5 .submit-btn{
        display: block;
        width: 100%;
        text-align: center;
        margin-top: 4vh;
        margin-bottom: 4vh;
    }
    .signUpH5 .submit-btn button{
        width: 40%;
        height: 4vh;
        margin: 0 auto;
        line-height: 4vh;
        text-align: center;
        color: #fff;
        font-size: 60px;
        border: none;
        background: #5588f2;
        border-radius: 20px;
    }
    /* 遮罩层 */
    .signUpH5 .Mask-box{
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
    .signUpH5 .epidemic-prevention-box{
        width: 100%;
        position: fixed;
        left: 0;
        top: 4vh;
        z-index: 1000;
    }
    .signUpH5 .epidemic-prevention{
        width: 62%;
        margin: 0 auto;
        text-align: center;
        position: relative;
    }
   .signUpH5 .epidemic-prevention-box .epidemic-preventionH5{
        width: 85%;
    }
    .signUpH5 .h5bgClick{
        position: absolute;
        width: 100%;
        height: 150px;
        bottom: 7%;
        left: 0;
        z-index: 10000;
    }
    .signUpH5 .epidemic-prevention img{
        width: 100%;
    }
    .signUpH5 .regulations{
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
    .signUpH5 .closeBtn{
        font-size: 6em;
        position: absolute;
        right: 2vw;
        top: 3vh;
        color: #dfdfdf;
        cursor: pointer;
    }
    .signUpH5 .epidemic-prevention-box .epidemic-preventionH5 .closeBtn{
        font-size: 6em;
        position: absolute;
        right: 2vw;
        top: 1vh;
        color: #dfdfdf;
        cursor: pointer;
    }
    .signUpH5 .epidemicCloseBtn{
         top: 90px;
    }
    .signUpH5 .index-bg{
        width: 83%;
        margin: 0 auto;
    }
    .signUpH5 .index-bg img{
        width: 100%;
        margin-top: 12%;
    }
    .signUpH5 .index-bg button{
        padding: 10px 32px;
        background: #5588f2;
        border: none;
        margin: 4vh 0;
        border-radius: 15px;
        color: #fff;
        font-size: 60px;
    }  
    .signUpH5 .Upload-file-tip p{
        font-size: 5.285714rem;
        margin-top: 3vh;
        font-weight: 700;
    }
    .signUpH5 .Upload-file{
        width: 100%;
        padding: 0 8vw;
        box-sizing: border-box;
        display: flex;
        margin-top: 3vh;
    }
    .signUpH5 .Upload-file .Upload-file-list{
        flex: 1;
        font-size: 4.285714rem;
        font-weight: 700;
    }
    .signUpH5 .Upload-file .Upload-file-list div{
        height: 10vh;
        line-height: 10vh;
        background: #bbbbbb;
        margin: 0 1vw;
        margin-top: 1vh;
        color: #fff;
        font-size: 4.285714rem;
        font-weight: 600;
    }
    .signUpH5 .Upload-file .Upload-file-list div img{
        width: 100%;
        height: 100%;
    }
    .signUpH5 .mask{
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: #000;
        opacity: 0.3;
        z-index: 10;
    }
    .signUpH5 .area1{
        background: #fff;
        z-index: 1000;
        width: 100%;
        height: 28%;
        position:fixed;
        left: 0;
        bottom: 0;
        overflow-y: scroll;
    }
    .signUpH5 .distpicker-address-wrapper .address-container ul li{
        font-size: 8pt;
        line-height: 5vh;
        color: #000;
    }
    .signUpH5 .distpicker-address-wrapper .address-header ul li{
        font-size: 8pt;
        line-height: 5vh;
        color: #000;
    }
    .signUpH5  .user-select label{
        font-size: 8pt;
        color: #000;
        font-weight: 700;
        float: left;
        line-height: 3vh;
    }
    .signUpH5  .user-select label:nth-of-type(1){
        margin-right: 5vw;
     }
   .signUpH5  .user-select input{
        width: 3vw;
        height: 3vh;
        margin-right: 1vw;
        float: left;
    }
    .signUpH5  .el-checkbox__label{
        font-size:4.285714rem;
        color: #000;
    }
    .signUpH5 .el-checkbox__inner{
        width: 5vw;
        height: 5vw;
    }
    .signUpH5 .el-checkbox__inner::after{
        box-sizing: content-box;
        content: "✓";
        border-left: 0;
        border-top: 0;
        left:15%;
        position: absolute;
        top: 15%;
        color: #fff;
        transform: rotate(15deg) scaleY(1) !important;
        -webkit-transform: rotate(
15deg) scaleY(1) !important;
        font-size: 10pt;
        border: none;
        
    }
    .Participate-in-sub-forums2{
        padding-left: 21vw;
        box-sizing: border-box;
        border: none;
    }
</style>