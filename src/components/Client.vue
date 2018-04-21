<template>
	<div>
	  <!-- <el-container>
	    <el-header>
	      供暖收费管理系统
	      <div class="header-info">
	        <div v-if="true">
	          <span>{{name}}已登录</span>
	          <a @click ='layout'>退出登陆</a>
	          <router-link to="/Login">退出登陆</router-link>
	        </div>
	        <div v-else>
	          <span>未登录</span>
	          <a>请先登陆</a>
	        </div>
	      </div>
	    </el-header>
	    <v-nav></v-nav> -->
	    <el-button type="text" @click="centerDialogVisible = true">新建用户</el-button>

		<el-dialog
		  title="请完善用户信息"
		  :visible.sync="centerDialogVisible"
		  width="50%"
		  center>
		  <!-- <span>需要注意的是内容是默认不居中的</span> -->
		  <el-form :model="ruleForm2" status-icon :rules="rules2" ref="ruleForm2" label-width="120px" class="demo-ruleForm reg-content" action="http://127.0.0.1:80/society/welcome/myreg" method="post">
                    <el-form-item label="姓名" prop="username">
                        <el-input type="text" v-model="ruleForm2.username" auto-complete="off" name="username"></el-input>
                    </el-form-item>
                    <el-form-item label="性别" prop="sex">
                      <el-radio-group v-model="sex" name="sex" class="radio-sex">
                        <el-radio label="男"></el-radio>
                        <el-radio label="女"></el-radio>
                      </el-radio-group>
                    </el-form-item> 
                    <el-form-item label="年龄" prop="age">
                        <el-input type="text" v-model="ruleForm2.age" auto-complete="off" name="age"></el-input>
                    </el-form-item>
                    <el-form-item label="电子邮箱" prop="email">
                        <el-input type="text" v-model="ruleForm2.email" auto-complete="off" name="email"></el-input>
                    </el-form-item>
                    <el-form-item label="手机号码" prop="telephone">
                        <el-input type="text" v-model="ruleForm2.telephone" auto-complete="off" name="telephone"></el-input>
                    </el-form-item>
                     <el-form-item label="地址" prop="address">
                        <el-input type="text" v-model="ruleForm2.address" auto-complete="off" name="address"></el-input>
                    </el-form-item>
                    <el-form-item label="在网状态" prop="balance_internet">
                      <el-radio-group v-model="balance_internet" name="balance_internet" class="radio-sex">
                        <el-radio label="入网"></el-radio>
                        <el-radio label="退网"></el-radio>
                        <el-radio label="拆户"></el-radio>
                        <el-radio label="过户"></el-radio>
                      </el-radio-group>
                    </el-form-item> 
                    <el-form-item label="用热状态" prop="balance_state">
                      <el-radio-group v-model="balance_state" name="balance_state" class="radio-sex">
                        <el-radio label="正常"></el-radio>
                        <el-radio label="停保"></el-radio>
                        <el-radio label="未供热"></el-radio>
                      </el-radio-group>
                    </el-form-item> 
                    <el-form-item label="用热方式" prop="balance_style">
                      <el-radio-group v-model="balance_style" name="balance_style" class="radio-sex">
                        <el-radio label="计量供热"></el-radio>
                        <el-radio label="面积供热"></el-radio>
                      </el-radio-group>
                    </el-form-item> 
                    <el-form-item>
                        <el-button type="primary" @click="submitForm('ruleForm2')">提交</el-button>
                        <el-button @click="resetForm('ruleForm2')">重置</el-button>
                    </el-form-item>
              </el-form>
		</el-dialog>
	  <!-- </el-container> -->
	</div>
</template>
<script>
import Axios from 'axios';
import nav from './NavMain'
  export default {
    components:{
          'v-nav': nav
      },
    data() {
    	var checkage = (rule, value, callback) => {
        if (value === '') {
          return callback(new Error('请输入年龄'));
        } else {
          callback();
        }
      };
      var validatePass = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入密码'));
        } else {
          if (this.ruleForm2.checkPass !== '') {
            this.$refs.ruleForm2.validateField('checkPass');
          }
          callback();
        }
      };
      var validatePass2 = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请再次输入密码'));
        } else if (value !== this.ruleForm2.pass) {
          callback(new Error('两次输入密码不一致!'));
        } else {
          callback();
        }
      };
      var checkUsername = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入姓名'));
        } else {
          callback();
        }
      };
      var checkstudyId = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入学号'));
        } else {
          callback();
        }
      };
      var checkIdEmail = (rule, value, callback) => {
        var regEmail = /^([0-9A-Za-z\-_\.]+)@([0-9a-z]+\.[a-z]{2,3}(\.[a-z]{2})?)$/g;
        if (value === '') {
          callback(new Error('请输入邮箱'));
        } else if (!regEmail.test(value)) {
          callback(new Error('请输入正确的邮箱'));
        } else {
          callback();
        }
      };
      var checkIdTelephone = (rule, value, callback) => {
        var regTelephone = /^[1][3,4,5,7,8][0-9]{9}$/;
        if (value === '') {
          callback(new Error('请输入电话号码'));
        } else if(value.length<11){
           callback(new Error('电话号码长度不足11位'));
        } else if(!regTelephone.test(value)) {
          callback(new Error('请输入正确的电话号码'));
        } else {
          callback();
        }
    }
    	var checkAddress = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入地址'));
        } else {
          callback();
        }
    }
      return {
        name: '你好',
        centerDialogVisible: false,
        tip: '',
        sex: '',
        balance_state: '',
        balance_style: '',
        balance_internet: '',
        ruleForm2: {
          username: '',
          age: '',
          email: '',
          telephone: '',
          address: ''
        },
        rules2: {
          username: [
            { validator: checkUsername, trigger: 'blur' }
          ],
          age: [
            { validator: checkage, trigger: 'blur' }
          ],
          email: [
            { validator: checkIdEmail, trigger: 'blur' }
          ],
          telephone: [
            { validator: checkIdTelephone, trigger: 'blur' }
          ],
          address: [
            { validator: checkAddress, trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      handleSelect(key, keyPath) {
        console.log(key, keyPath);
      },
      layout: function() {
        localStorage.clear();
        this.$router.push('./login');
      },
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
          	var myDate = new Date();
          	var mytime=myDate.toLocaleTimeString();     //获取当前时间
          	console.log(myDate);
          	console.log(mytime);
          	console.log(myDate.toLocaleString());
            var params = new URLSearchParams(); 
                params.append('username',this.ruleForm2.username);
                params.append('sex',this.sex);
                params.append('age',this.ruleForm2.age);
                // params.append('age','20');
                params.append('email',this.ruleForm2.email);
                params.append('telephone',this.ruleForm2.telephone); 
                params.append('address',this.ruleForm2.address); 
                params.append('entertime',myDate);
                params.append('balance_internet',this.balance_internet);
                params.append('balance_state',this.balance_state);
                params.append('balance_style',this.balance_style);
               
                Axios.post('http://127.0.0.1:80/heatphp/welcome/create_user',params).then((res)=>{
                  if(res){
                  	console.log(res);
                    this.centerDialogVisible = true;
                    // this.tip = TIPS[res.data];
                    var _this = this;
                    if(res.data === 1) {
                      this.centerDialogVisible = false;
                     //  setTimeout(function(){
                     //    _this.$router.push('./Login');
                     // }, 2000);
                    } else {
                      this.centerDialogVisible = true;
                      //alert('fail');
                    }
                  }
              });
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      }
     }
  }
</script>
<style scoped>
  a {
    text-decoration: none;
  }

  .el-carousel {
    overflow: hidden;
    width: 960px;
    margin: 0 auto;
    padding-top: 100px;
    min-height: 350px;
    /*background: #324057;*/
  }

  .el-carousel__item h3 {
    color: #475669;
    font-size: 14px;
    opacity: 0.75;
    line-height: 200px;
    margin-top: 0;
  }

  .el-carousel__item:nth-child(2n) {
    background-color: #99a9bf;
  }

  .el-carousel__item:nth-child(2n+1) {
    background-color: #d3dce6;
  }

  .el-carousel__item img {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1;
    width: 100%;
    height: 300px;
  }

  .header-info {
    float: right;
    height: 60px;
    overflow: hidden;
    font-size: 2px;
  }

  .header-info div {
    display: inline;
    cursor: pointer;
  }

  .header-info a:hover {
    color: #409EFF;
  }

  .el-header, .el-footer {
    background-color: #B3C0D1;
    color: #333;
    text-align: center;
    line-height: 60px;
  }

  .el-main {
    background-color: #E9EEF3;
    color: #333;
    text-align: center;
    height: 650px;
  }

  body > .el-container {
    margin-bottom: 40px;
  }

  .el-container:nth-child(5) .el-aside,
  .el-container:nth-child(6) .el-aside {
    line-height: 260px;
  }

  .el-container:nth-child(7) .el-aside {
    line-height: 320px;
  }
</style>
