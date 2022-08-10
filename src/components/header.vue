<template>
    <div class="content">
      <div class="header-list">
          <div class="list1"><img src="@/assets/img/ossChange/IMT-2020-logo.png" alt="" srcset=""></div>
          <div class="list2">
            <input type="text" name=""  placeholder="请输入关键字" id=""/>
            <el-button slot="append" icon="el-icon-search">站内搜索</el-button>
          </div>
      </div>

      <div class="menu-list">
        <div class="list">
            <p v-bind:class="{ active: isActive=='1'}">
              <router-link :to="{path:'/index',query: {name: 1}}">首页</router-link>
            </p>
            <p v-bind:class="{ active: isActive=='2' }">
              <router-link :to="{path:'/conferenceDetail',query: {name: 2}}">大会概括</router-link>
            </p>
            <p v-bind:class="{ active: isActive=='3' }">
              <router-link :to="{path:'/newsCenter',query: {name: 3}}">会议日程</router-link>
            </p>
            <p v-bind:class="{ active: isActive=='4' }">
              <router-link :to="{path:'/tradeFair',query: {name: 4}}">直播回放</router-link>
            </p>
            <p v-bind:class="{ active: isActive=='8' }">
              <router-link :to="{path:'/registration',query: {name: 8}}">报名注册</router-link>
            </p>
            <p v-bind:class="{ active: isActive=='9' }"><router-link :to="{path:'/contactUs',query: {name: 9}}">联系我们</router-link></p>
        </div>
      </div>

    </div>
</template>

<script>
export default {
  name: 'Header',
  props: {
    msg: String
  },
  data(){
    return{
        isActive:'1'
    }
  },
  mounted(){
    var query=this.$route.query;
    let name = query.name;
    sessionStorage.setItem('name',name);
    this.isActive = name || '1';
  },
  methods:{

  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped >
.content{
  width: 100%;
  background: #fff;
  border-bottom: 1px solid #dedede;
  z-index: 1000;
} 
.header-list{
    width: 1200px;
    margin: 0 auto;
    display: flex;
    height: 108px;
}
.header-list div{
  flex: 1;
}
.header-list .list1{
  text-align: left;
}
.header-list .list1 img{
  width: 347px;
  margin-top: 23px;
}

.header-list .list2 {
  text-align: right;
  padding-top: 35px;
  box-sizing: border-box;
}
.header-list .list2 input{
  width: 164px;
  height: 38px;
  border: 1px solid #dedede;
  padding-left: 7px;
  box-sizing: border-box;
  border-top-left-radius: 10px;
  border-bottom-left-radius: 10px;
  border-top-right-radius: none;
  border-bottom-right-radius: none;
  font-size: 14px;
  outline-color:#1e7be2;
}
.header-list .list2 input:active{
  border: none;
}
.header-list .list2 button{
  width: 90px;
  height: 40px;
  text-align: left;
  float: right;
  background: #5588f2;
  color: #fff;
  font-size: 12px;
  padding: 0;
  padding-left: 6px;
  box-sizing: border-box;
  margin-top: -1px;
}
@media (width: 1280px) {
  .header-list .list2 button{
    width: 120px;
  }
  .header-list .list2 input{
    width: 180px;
  }

} 


.menu-list{
    width: 100%;
    height: 80px;
    background: #fff;

}
.menu-list .list{
    display: flex;
    width: 1200px;
    margin: 0 auto;
    height: 80px;
    line-height: 80px;
    border-top: 1px solid #dedede;
}
.menu-list p{
    flex: 1;
    font-size: 18px;
    text-align: center;
    cursor: pointer;
    position: relative;
}
.menu-list .active a{
    color: #5387f2;
}
.menu-list .active:hover .ul-list{
  visibility:visible;
}

.menu-list  .ul-list{
  visibility: hidden;
  position: absolute;
  left: -45px;
  top: 55px;
  background: #5387f2;
  width: 160px;
  height: auto;
  text-align: center;
  box-sizing: border-box;
  padding: 15px 0;
  border-radius: 5px;
}
.menu-list .ul-list li{
  height:34px;
  line-height: 34px;
}
.menu-list .ul-list li a{
 color: #fff;
 font-size: 16px;
 text-align: center;
}
</style>
