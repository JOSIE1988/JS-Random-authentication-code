<template>
  <div>
    <div class="login_or_register">
      <div class="login_content">
        <div class="login_bac ">
          <div class="login_content1 ">
            <p><img src="/static/img/suo.png" class="tianxie_img">我已有账号，登录</p>
            <input type="text" placeholder="请输入手机号" class="register_content_input" v-model= "LUserPhone" @blur="checkLPhone"><br>
            <span class="tishixiaoxi disappear">请输入手机号。</span>
            <input type="password" placeholder="请输入密码" class="register_content_input" v-model="LUserPsd" @blur="checkLPsd"><br>
            <span class="tishixiaoxi disappear">请输入密码。</span>
            <input type="text" placeholder="请输入验证码" class="yanzhengma_input" @blur="checkLpicma" v-model="picLyanzhengma"><input type="button" id="code" @click="createCode"  class="verification1" v-model="checkCode"/> <br>
            <span class="tishixiaoxi disappear">请输入验证码。</span>
            <a class="user_login" @click="Login">登录</a>
          </div>
        </div>
      </div>
    </div>
  </div>
  
  
</template>

<script>
var code ; //在全局定义验证码
export default {
    data () {
      return {
        userPhone:'',
        dialog: false,
                LUserPhone:'',
                LUserPsd:'',
                picLyanzhengma:'',
                checkCode:''
      }
    },
    methods:{
      goto_protocol(){
        this.$router.push({path:"/protocol"})
        
      },
      checkUserPhone(){
        if(this.userPhone == ''){
          console.log(111)
          $(".hiddenTanchuang").removeClass('hiddenTanchuang')
        }
      },
      hiddenTanchuang(){
        $(".tanchuang").addClass("hiddenTanchuang")
      },
        
      // 验证登陆手机号格式
      checkLPhone(){
          if(this.LUserPhone == ''){
              $(".login_content1 span:eq(0)").removeClass("disappear");
              $(".login_content1 span:eq(0)").text("请输入手机号。")

          }else if(this.LUserPhone.search(/^(0|86|17951)?(13[0-9]|15[012356789]|17[678]|18[0-9]|14[57])[0-9]{8}$/)==0){
              $(".login_content1 span:eq(0)").addClass("disappear")
              return true;

          }else{
              $(".login_content1 span:eq(0)").removeClass("disappear");
              $(".login_content1 span:eq(0)").text("请输入正确手机号。")
          }
      },
      //验证登陆密码格式
      checkLPsd(){
          if(this.LUserPsd == ''){
              $(".login_content1 span:eq(1)").text("请输入密码");
              $(".login_content1 span:eq(1)").removeClass("disappear")

          }else if(this.LUserPsd.search(/^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]{6,20}$/) == 0){
              $(".login_content1 span:eq(1)").addClass("disappear")
              return true;


          }else{
              $(".login_content1 span:eq(1)").removeClass("disappear");
              $(".login_content1 span:eq(1)").text("密码必须6-20位，包含字母与数字")

          }
      },
      // 图片验证码
      createCode(){
          code = "";    
          var codeLength = 4;//验证码的长度   
          var random = new Array(0,1,2,3,4,5,6,7,8,9,'A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R',   
           'S','T','U','V','W','X','Y','Z');//随机数   
          for(var i = 0; i < codeLength; i++) {//循环操作   
              var index = Math.floor(Math.random()*36);//取得随机数的索引（0~35）   
              code += random[index];//根据索引取得随机数加到code上   
          }   
              this.checkCode = code;//把code值赋给验证码   
      },
      // 失焦验证图和密码
      checkLpicma(){
        
          this.picLyanzhengma.toUpperCase();//取得输入的验证码并转化为大写         
          if(this.picLyanzhengma == '') {
              $(".login_content1 span:eq(2)").text("请输入验证码")
              $(".login_content1 span:eq(2)").removeClass("disappear");
             
          }else if(this.picLyanzhengma.toUpperCase() != this.checkCode ) { //若输入的验证码与产生的验证码不一致时    
              console.log( this.picLyanzhengma.toUpperCase())
              console.log(code)           
              $(".login_content1 span:eq(2)").text("验证码不正确")
              $(".login_content1 span:eq(2)").removeClass("disappear");
              this.createCode();//刷新验证码   
              this.picLyanzhengma = '';
          }else { //输入正确时   
              $(".login_content1 span:eq(2)").addClass("disappear");
              $(".login_content1 span:eq(2)").text("请输入验证码")
              return true;

          } 

      },
      Login(){
          if(this.checkLPhone() ==true && this.checkLPsd() == true && this.checkLpicma() == true){
              var that = this;
              $.ajax({
                  type:"POST",
                  url:this.HOST+"/user/logincheck",
                  data:{"loginmobileNo":this.LUserPhone,"loginpassword":this.LUserPsd},
                  dataType:"json",
                  success:function(data){
                      console.log(data);
                      if(data.resultflag == "F"){
                          $(".login_content1 span:eq(0)").removeClass("disappear");
                          $(".login_content1 span:eq(0)").text("手机号或密码错误。")
                      }else{
                         that.$router.push({path:"/brain-assetList1"})
                      }
                  }
              })
          }
      } 

},
    created(){
        this.createCode();
    }
}
</script>

<style scoped>
/*@import '/static/css/register_login.css'*/
.left{
    float: left;
}
.right{
    float: right;
}
.clearfix:after{
    content:'';
    display:block;
    height:0;
    clear:both;
}
.login_or_register{
    background-color: #f2f2f5;
    padding-bottom: 50px;
}
/*logo*/
.logo1{
    text-align: center;
    padding:40px 0 50px ;
    margin:0 auto;
}
/*login和register部分*/
.login_bac{
    width:1180px;
    height: 728px;
    background-size: 1180px 728px;
    margin: 0 auto;
    position: relative;
}
.register_content{
    width:400px;
    background-color: #fff;
    box-shadow: 0 0 15px rgba(0, 0, 0, .5);
    position: absolute;
    left:60px;
    top:80px;
    padding-left: 30px;
    padding-bottom: 50px;
}
.tianxie_img{
    width:26px;
    vertical-align: middle;
    margin-right: 10px;
}
.tishixiaoxi{
    font-size: 14px;
    color:#ed711f;
    display: block;
    margin-left: 30px;
    line-height: 30px;
   
}
.yanzhengma_input{
  width: 170px;
}
.disappear{
     visibility:hidden;
}
.register_content p{
    margin-top: 30px;
    font-size: 25px;
    line-height: 18px;
    margin-bottom: 15px;
}
.register_content input{
    padding: 5px 0 5px 10px;
    margin-left: 30px;
    height: 30px;
    /*margin-top: 25px;*/
    border: 1px solid #e6e6e6;
}
.register_content_input{
    width:300px;
    height: 30px;
    padding: 5px 0 5px 10px;
    /*margin-top: 30px;*/
    border: 1px solid #e6e6e6;
    margin-left: 30px;
}
.verification{
    vertical-align: middle;
    margin-left: 10px;
}
.agreement{
    position: relative;
    top: -10px;
}
.read_already{
    position: relative;
    top:-21px;
    font-size: 12px;
    color: #999;
    cursor: pointer;
    margin-left: 5px;
}
.read_already a{
    color:#053d84;
}
.send_message{
    display: inline-block;
    height: 40px;
    line-height: 40px;
    color:#ccc;
    padding-left: 45px;
    padding-right: 5px;
    background: url('/static/img/message.png') no-repeat 10px 8px;
    background-size: 25px;
    border: 1px solid #f3f3f3;
    margin-left: 10px;
    cursor: pointer;
}
.next{
    display: block;
    width: 287px;
    height: 50px;
    font-size: 18px;
    text-align: center;
    line-height: 50px;
    color: #fff;
    background-color: #053d84;
    border-radius: 5px;
    margin-left: 40px;
    cursor: pointer;

}
/*登录模块*/
.login_content1{
    width:400px;
    background-color: #fff;
    box-shadow: 0 0 15px rgba(0, 0, 0, .5);
    position: absolute;
    right:60px;
    top:80px;
    padding-left: 30px;
    padding-bottom: 50px;
}
.user_login{
    display: block;
    width: 287px;
    height: 50px;
    font-size: 18px;
    text-align: center;
    line-height: 50px;
    color: #fff;
    background-color: #a2be3c;
    border-radius: 5px;
    margin-left: 40px;
    /*margin-top: 30px;*/
    cursor:pointer;
}
.login_content1 p{
    margin-top: 30px;
    font-size: 25px;
    line-height: 18px;
    margin-bottom: 15px;
}
.login_content1 input{
    padding: 5px 0 5px 10px;
    margin-left: 30px;
    height: 30px;
    /*margin-top: 25px;*/
    border: 1px solid #e6e6e6;
}
.verification1{
    vertical-align: middle;
    transform: translate(-15px,0);
    width: 102px;
}
.forget{
    text-align: right;
    position: relative;
    top:-20px;
    cursor: pointer;
}
.forget a{
    font-size: 14px;
    color:#053d84;
    margin-right: 80px;
}
#code{
    font-size: 18px;
    letter-spacing:3px;
    color: #053d84;
    background: #f2f2f5;
}
/*弹窗*/
.tanchuang{
    position: fixed;
    top:0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color:rgba(0,0,0,.5);
    z-index:100;
}
.hiddenTanchuang{
    display: none;
}
.inTanchuang{
    position: absolute;
    text-align: center;
    width: 300px;
    height: 169px;
    padding-top: 10px;
    left: 50%;
    top: 50%;
    border-radius: 10px;
    -webkit-transform: translateX(-50%) translateY(-50%);
    -ms-transform: translateX(-50%) translateY(-50%);
    transform: translateX(-50%) translateY(-50%);
    background-color: #fff;
}
.errorIcon{
    width:55px;
}
.register_content  .cue{
    font-size: 14px;
    text-align: center;
    margin-top: 15px;
}
.sure{
    display: inline-block;
    color: #fff;
    width: 100px;
    height: 20px;
    padding: 6px 10px;
    background: #053D84;
    border-radius: 5px;
    font-size: 18px;
    line-height: 18px;
    margin-top: 10px;
    cursor: pointer;
}

</style>
