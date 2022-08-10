<template>
  <div id="app">
    <!-- <div class="postpone" id="postpone">大会延期 时间待定</div> -->
    <router-view></router-view>
  </div>
</template>

<script>
  export default {
    name: 'App',
    data(){
      return{
      
      }
    },
    mounted(){
        window.alert = function(name){
            var iframe = document.createElement("IFRAME");
            iframe.style.display="none";
            iframe.setAttribute("src", 'data:text/plain');
            document.documentElement.appendChild(iframe);
            window.frames[0].window.alert(name);
            iframe.parentNode.removeChild(iframe);
        }
    },
    methods:{
      
    }
  }
</script>
<style>
html,body{
  width: 100%;
  height: 100%;
  overflow: scroll;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #303030;
  width: 100%;
  z-index: 10;
  
}
*{
  margin: 0;
  padding: 0;
}
body,h1,h2,h3,h4,h5,h6,div,dd,dt,dl,form,kbd,table,ul,li,marquee,hr,blockquote,ol,pre,fieldset,legend,button,input,textarea,th,td{font-size:12px; margin:0; padding:0; font-family: "微软雅黑",Arial, "宋体", Tahoma, Verdana,  Helvetica, sans-serif;}
address,cite,dfn,em,var{font-style:normal;}
button,input,select,textarea{font-size:100%; cursor:pointer}
input{ cursor:pointer}
table{border-collapse:collapse;border-spacing:0;}
small{font-size:12px;}
ul,ol{list-style:none;}
img{border:0;}
ul{list-style:none;}
.clear{ clear:both;}
a{text-decoration: none; color: #666666; }
a:hover{ color:#000; text-decoration:none}
input[type="submit"],input[type="reset"],input[type="button"],input[type="text"], button { -webkit-appearance: none;border-radius: 0; } 

.clearfix:after { visibility: hidden; display: block; font-size: 0; content: " "; clear: both; height: 0; }
::-webkit-scrollbar {
  display: none; /* Chrome Safari */
  -ms-overflow-style: none; /* IE 10+ */
  scrollbar-width: none; /* Firefox */  
}

.shuiyin-bg-box{
    width: 100%;
    background: url('./assets/img/ossChange/shuiying-bg-appPage.png') 0 1% no-repeat;
    background-size: cover;
}
.index-bg{
  z-index: 100;
  box-sizing: border-box;
}
.el-message{
  width: 40%;
  text-align: center;
  height: 4vh;
  font-size: 8rem !important;
}
.el-message p{
  font-size: 5rem !important;
}
/* .postpone{
  width:40vw;
  height: 8vw;
  color: red;
  font-size: 4vw;
  font-weight: 700;
  background: #68c7ff;
  position: fixed;
  top: 40%;
  left: 40%;
  z-index: 10000;
  line-height:8vw;
  margin: 0 auto;
  animation: runOne 10s linear infinite;
}
@keyframes runOne {
  from {
      left: -30%;
  }

  to {
      left: 100%;
  }
} */


</style>
