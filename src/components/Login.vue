<template>
    <div class="wrap">
        <el-row>
            <div class="index-img">
              <el-col :span="8" class="content-right">
                <h1 class="title">欢迎登录供暖管理系统</h1>
                <el-form :model="ruleForm2" status-icon :rules="rules2" ref="ruleForm2" label-width="100px" class="demo-ruleForm" action="http://127.0.0.1:80/heatphp/welcome/login" method="post">
                    <el-form-item label="用户名" prop="username">
                        <el-input type="text" v-model="ruleForm2.username" auto-complete="off" name="username"></el-input>
                    </el-form-item>
                    <el-form-item label="密码" prop="pass">
                        <el-input type="password" v-model="ruleForm2.pass" auto-complete="off" name="password"></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" @click="submitForm('ruleForm2')">提交</el-button>
                        <el-button @click="resetForm('ruleForm2')">重置</el-button>
                    </el-form-item>
                </el-form>
             </el-col>
            </div>
            
        </el-row>
            <el-dialog
              title="提示"
              :visible.sync="centerDialogVisible"
              width="30%"
              center>
              <span>您的用户名或者密码有误，请确认后重新登录</span>
              <span slot="footer" class="dialog-footer">
                <el-button @click="centerDialogVisible = false">取 消</el-button>
                <el-button type="primary" @click="centerDialogVisible = false">确 定</el-button>
              </span>
            </el-dialog>
    </div>
  
</template>
<script>
import Axios from 'axios';
    export default {
        created() {
            console.log(localStorage);
        },
        data() {
          var validatePass = (rule, value, callback) => {
            if (value === '') {
                callback(new Error('请输入密码'));
            } else {
                callback();
            }
          };
          var checkusername = (rule, value, callback) => {
            if (value === '') {
              callback(new Error('请输入用户名'));
            } else {
              callback();
            }
          };
          return {
            ruleForm2: {
              username: '',
              pass: ''
            },
            rules2: {
              username: [
                { validator: checkusername, trigger: 'blur' }
              ],
              pass: [
                { validator: validatePass, trigger: 'blur' }
              ]
            },
            centerDialogVisible: false
          };
        },
        methods: {
          submitForm(formName) {
            this.$refs[formName].validate((valid) => {
              if (valid) {
                var params = new URLSearchParams();
                params.append('username',this.ruleForm2.username);
                params.append('password',this.ruleForm2.pass);
                Axios.post('http://127.0.0.1:80/heatphp/welcome/login',params).then((res)=>{
                 if (res.data !== 0) {
                    localStorage.setItem('username', res.data.admin_username);
                    localStorage.setItem('id', res.data.admin_id);
                    this.$router.push('./home');
                 } else {
                    this.centerDialogVisible = true;
                 }
                });
              } else {
                // this.centerDialogVisible = true;
                // return false;
              }
            });
          },
          resetForm(formName) {
            this.$refs[formName].resetFields();
          }
        }
    }
</script>
<style>
  .wrap {
    width: 1170px;
    margin: 0 auto;
  }
  .index-img {
    background-image: url(../assets/login_bg.png);
    height: 500px;
    /*width: 70%;*/
  }
  .el-row { 
    margin-top: 120px;
    margin-bottom: 20px;
  }
  .el-col {
    border-radius: 4px;
  }
  .title {
    color: #409EFF;
    margin: 50px 0 50px 50px;
  }
  a {
    text-decoration: none;
  }
  .forget-reg {
    color: #409EFF;
    font-size: 12px;
    float: right;
  }
  .content-right {
    margin-top: 80px;
    margin-right: 80px;
    float: right;
  }
</style>