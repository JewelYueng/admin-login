<template>
  <div>
    <div id="login-box">
      <div id="title">管理员</div>
      <el-form :model="ruleForm" :rules="rulesInput" ref="ruleForm" label-width="60px"
               id="LoginForm">
        <el-form-item label="工号" prop="workId">
          <el-input id="workId" v-model="ruleForm.workId" style="width: 240px;right: 10px"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input type="password" v-model="ruleForm.password" style="width: 240px;right: 10px"></el-input>
        </el-form-item>
        <el-form-item label="验证码" prop="validateCode" class="code">
          <el-input size="small" id="checkCode" v-model="ruleForm.validateCode"
                    style="width: 40%;right: 30px;vertical-align: top">
          </el-input>
          <div class="img">
            <div>
              <img :src='imgUrl' alt="验证码">
            </div>
            <div>
              <a @click="changeCode">看不清楚？</a>
            </div>
          </div>
        </el-form-item>

      </el-form>
      <div  class="login-btn">
        <el-button type="primary" @click="login()">登陆</el-button>
      </div>
    </div>
  </div>
</template>
<style lang="less" rel="stylesheet/less">
  #login-box {
    height: 290px;
    width: 300px;
    border-radius: 5px;
    box-shadow: 0 0 1px 0 #8492a6;
    border: 1px solid #8492a6;
    padding: 20px;
    margin: 100px auto;
    background-color: white;
  }

  #title {
    font-size: 16px;
    padding-top: 5px;
    padding-bottom: 6px;
    margin-bottom: 20px;
  }

  .el-form-item__label {
    text-align: left;
  }


  .code {
    .el-form-item__error{
      top: 60%;
    }
    .img{
      width: 100px;
      height: 56px;
      overflow: hidden;
      div{
        height: 28px;
      }
      display: inline-block;
      a{
        position: relative;
        text-decoration: underline;
        cursor: pointer;
        color: #5f6e96;
      }
    }
  }
  .auto-login{
    .el-form-item__content{
      position: relative;
      left: -60px;
      text-align: left;
      top:-30px;
      color: gray;
    }
  }
  .login-btn{
    position: relative;
    margin: 20px;
    top:-20px;
    button{
      width: 100px;
    }
  }

</style>
<script>
export default {
  name: 'hello',
  data(){
    let checkCode = (rule, value, callback) => {
      if (!value) {
        return callback(new Error('验证码不能为空'));
      } else {
        callback();
      }

    };
    let validateEmail = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入工号'));
      } else {
        callback();
      }
    };
    let validatePass = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入密码'));
      } else {
        callback();
      }
    };
    return {
      imgUrl: '',
      ruleForm: {
        workId: '',
        password: '',
        validateCode: '',
      },
      rulesInput: {
        workId: [
          {validator: validateEmail, trigger: 'blur'}
        ],
        password: [
          {validator: validatePass, trigger: 'blur'}
        ],
        validateCode: [
          {validator: checkCode, trigger: 'blur'}
        ]
      }
    };

  },
  created(){
    this.imgUrl = "/AssWeCan/home/code?date=" + Date()
  },
  methods: {
    changeCode(){
      this.imgUrl = "/AssWeCan/home/code?date=" + Date()
    },
    login(){
      this.$refs.ruleForm.validate((valid) => {
        if (valid) {
          this.$http.post('/AssWeCan/admin/adminLogin', this.ruleForm,{
            headers: {
              "Content-Type": 'application/json'
            }
          }).then(res => {
            if(res.data.code === "200") {
              this.$message({
                message: '登陆成功',
                type: 'success'
              });
              window.location.href = '/AssWeCan/admin/home'
            }else {
              this.$message({
                message: res.data.message,
                type: 'error'
              });
              this.imgUrl = "/AssWeCan/home/code?date=" + Date()
            }

          }, err => {
            if(err.status !== 404) {
              this.$message({
                message: err.data.message,
                type: 'error'
              });
              this.imgUrl = "/AssWeCan/home/code?date=" + Date()
            }
          })
        } else {
          this.$message({
            message: '请正确填写表单信息',
            type: 'error'
          });
          this.imgUrl = "/AssWeCan/home/code?date=" + Date()
        }
      })
    },

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
