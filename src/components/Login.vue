<template>
  <div class="main" v-loading="loading">
    <div class="header">
      <img src="../assets/img/logo.png" alt="#" class="logo">
      <div class="header-button">
        <el-button round style="margin-right: 20px" plain>登录</el-button>
        <el-button round plain>返回首页</el-button>
      </div>
    </div>
    <div class="box">
      <h1>问卷星登录</h1>
      <ul style="padding: 0 50px 15px 50px">
        <li style="margin-bottom: 30px;">
          <el-input v-model="userName" placeholder="用户名" prefix-icon="el-icon-user" clearable></el-input>
        </li>
        <li>
          <el-input v-model="psw" placeholder="密码" prefix-icon="el-icon-lock" clearable></el-input>
        </li>
      </ul>
      <button class="submitButton" @click="Login">登录</button>
      <a href="#">立即注册</a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data() {
    return {
      loading:false,
      userName: '',
      psw: '',
    };
  },
  methods: {
    Login() {
      this.$axios.get('http://localhost:8080/user/login', {
        params: {
          name: this.userName,
          psw: this.psw
        }
      }).then(res => {
        if(res.data.result===1){
          this.$store.commit("setId",res.data.id)
          this.$store.commit("setName",res.data.name)
          sessionStorage.setItem("store",JSON.stringify(this.$store.state))
          this.$router.push('/main')
        }
        else{
          this.$message({
            message: '账号或密码错误！',
            type: 'warning'
          });
        }
      })
    }
  },
  created() {
    this.loading=true
    setTimeout(()=>{
      this.loading=false
    },400)
  }
};
</script>
<style scoped>
.main {
  height: 100%;
  background-image: url("../assets/img/background.jpg");
  background-size: 100% 100%;
  text-align: center;
}

.header {
  height: 95px;
  padding: 0 60px 0;
}

.logo {
  float: left;
  padding-top: 35px;
}

.header-button {
  float: right;
  height: 40px;
  width: 250px;
  padding-top: 40px;
}

.box {
  margin: 120px auto;
  background-color: white;
  width: 480px;
  height: 450px;
  border-radius: 14px;
}

.box h1 {
  text-align: center;
  padding: 40px 0 40px 0;
  color: darkorange;
}

.submitButton {
  display: block;
  width: 380px;
  height: 50px;
  margin: 30px auto 40px;
  border-radius: 30px;
  border: none;
  font-size: 18px;
  letter-spacing: 2px;
  color: white;
  background-color: darkorange;
}

.submitButton:hover {
  background-color: #ef8500;
  cursor: pointer;
}

.box a {
  font-size: 18px;
  color: darkorange;
  text-decoration: none;
}

.box a:hover {
  color: #ea8400;
}
</style>
