<template>
  <div class="login">
    <div v-title>登录</div>
    <div class="login-tou">
      <img src="../../static/login-t.png" alt="">
    </div>
    <div class="login-content">
      
      <div class="login-yan">
        <div class="login-yan-left">
          <input  v-model="telphone" placeholder="请输入手机号" class="number-input" @blur="outPoint()" @focus="focusPoint()">
        </div>
        <!-- <div class="login-yan-right">
          <mt-button size="small" type="primary" :disabled="isPort" @click="getPort()">{{huoContent}}</mt-button>
        </div> -->
      </div>
      <div class="login-tel" >
        <input  v-model="yanNum" placeholder="请输入密码" class="number-input" @blur="outPoint()" @focus="focusPoint()">
      </div>
      <div style="font-size: 12px;display: inline-block; width: 100%;text-align: right" ><span @click="jumpToforget()">忘记密码</span> </div>
    </div>
    <mt-button type="primary" class="login-button" @click="login()">登录</mt-button>
    <span class="registration" @click="jumpTo()" :style="{'display': isShow ? 'block':'none'}">注册</span>
  </div>
</template>
<script>
import { MessageBox } from 'mint-ui'
export default {
  name: 'login',
  data() {
    return {
      isPort: false,
      huoContent: '获取验证码',
      yanNum:'',
      telphone: '',
      isShow: true,
      timeout: ''
    }
  },
  methods: {
    jumpToforget: function () {
      this.$router.push({
        path: '/Forget'
      })
    },
    outPoint: function() {
      this.isShow = true
      this.timeout = setTimeout(() => {
        document.body.scrollTop = 0
      }, 500)
    },

    focusPoint: function () {
      this.isShow = false
      clearTimeout(this.timeout)
    },
    // getPort: function() {
    //   if (!this.telphone) {
    //     MessageBox('提示', '您还未输入手机号')
    //     return false
    //   } else {
    //     if(!(/^1[345789]\d{9}$/.test(this.telphone))){ 
    //       MessageBox('提示', '手机号码有误，请重填');  
    //       return false;
    //     } else {
    //       this.isPort = true
    //       let num = 61
    //       let daojishi = setInterval(() => {
    //         num--
    //         this.huoContent = num + 's后重新获取'
    //         if (num < 0) {
    //           clearInterval(daojishi)
    //           this.isPort = false
    //           this.huoContent = '获取验证码'
    //         }
    //       }, 1000)
    //       this.axios({
    //         method: 'post',
    //         url: '/api/getVerificationCode',
    //         headers: {
    //           'Content-type': 'application/json;charset=UTF-8'
    //         },
    //         data: {
    //           userPhone: this.telphone
    //         }
    //       }).then((res) => {
    //       })
    //     }
    //   }
    // },
    jumpTo: function() {
      this.$router.push({
        path: '/registration'
      })
    },
    login: function() {
      if (this.telphone && this.yanNum) {
        this.axios({
          method: 'post',
          url: '/api/postLoginSubmit',
          headers: {
            'Content-type': 'application/json;charset=UTF-8'
          },
          data: {
            telPhone: this.telphone,
            password: this.yanNum
          }
        }).then((res) => {
          if (res.data.code == 200 || res.data.code == '200') {
            window.localStorage.setItem('userMsg', JSON.stringify(res.data.data))
            this.$router.push({
              path: '/home'
            })
          } else {
            MessageBox({
              title: '小提示',
              message: res.data.msg,
            })
          }
        })
      }
    }
  }
}
</script>
<style scoped>
.login {
  width: 100%;
  height: 100vh;
  background-color: #fff;
}
.login-tou {
  padding-top: 40px;
  margin: auto;
  text-align: center;
  padding-bottom: 40px;
}
.login-tou img {
  width: 100px;
  height: 100px;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(215, 215, 215, 1);
}

.login-content {
  width: 80%;
  box-sizing: border-box;
  padding: 10px;
  margin: auto;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(215, 215, 215, 1);
}
.login-tel {
  height: 50px;
  box-sizing: border-box;
  margin-bottom: 10px;
  padding-top: 10px;
}
.login-tel input {
  width: 100%;
  height: 100%;
  border: 1px solid #fff;
  border-bottom: 1px solid #e7e8ed;
  font-size: 16px;
  outline:none;
}
.login-yan {
  height: 50px;
  box-sizing: border-box;
  border-bottom: 1px solid #e7e8ed;
  overflow: hidden;
  padding-top: 10px;
}
.login-yan-left {
  float: left;
  width: 50%;
  height: 100%
}
.login-yan-left input {
  width: 100%;
  height: 100%;
  font-size: 16px;
  outline:none;
}
.login-yan-right {
  float: right;
  padding-right: 5px;
  width: 45%;
  text-align: right;
}
.login-button {
  width: 80%;
  margin-left: 10%;
  margin-top: 50px;
}
.registration {
  width: 100px;
  position: fixed;
  display: inline-block;
  bottom: 20px;
  text-decoration: none;
  color: #5dd5ca;
  text-align: center;
  left: calc(50% - 50px);
}
</style>

