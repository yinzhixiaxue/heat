<template>
	<div>
	  <el-button type="text" @click="createUser">新建用户</el-button>
    <el-form>
        <el-form-item>
          <el-input v-model="input" placeholder="请输入姓名" class="input"></el-input> 
          <el-button type="primary" plain class="select" @click="select">搜索</el-button>
        </el-form-item>
      </el-form>
     <el-table
      ref="multipleTable"
      :data="tableData3"
      tooltip-effect="dark"
      style="width: 100%"
      @selection-change="handleSelectionChange">
<!--       <el-table-column
        type="selection"
        width="55">
      </el-table-column> -->
      <el-table-column
        prop="user_id"
        label="id"
        width="80">
        <template slot-scope="scope">{{ scope.row.user_id }}</template>
      </el-table-column>
      <el-table-column
        prop="user_name"
        label="姓名"
        width="120">
      </el-table-column>
      <el-table-column
        prop="user_sex"
        label="性别"
        width="80">
      </el-table-column>
      <el-table-column
        prop="user_age"
        label="年龄"
        width="80">
      </el-table-column>
      <el-table-column
        prop="user_telephone"
        label="联系方式"
        width="120">
      </el-table-column>
      <el-table-column
        prop="user_address"
        label="地址"
        width="200"
        show-overflow-tooltip>
      </el-table-column>
      <el-table-column
        prop="user_entertime"
        label="入网时间"
        width="120">
      </el-table-column>
      <el-table-column
        prop="balance_state"
        label="用热状态"
        width="120">
      </el-table-column>
      <el-table-column
        prop="balance_style"
        label="用热方式"
        width="120">
      </el-table-column>
      <el-table-column
        fixed="right"
        label="操作"
        width="300">
        <template slot-scope="scope">
          <el-button
            @click.native.prevent="Split_user(scope.row.user_id, scope.row.user_name)"
            type="text"
            size="small">
            拆迁客户管理
          </el-button>
          <el-button type="text" size="small" @click.native.prevent="Transfer_user(scope.row.user_id, scope.row.user_name)">客户过户管理</el-button>
        </template>
      </el-table-column>
    </el-table>
		<el-dialog
		  title="请完善用户信息"
		  :visible.sync="centerDialogVisible"
		  width="50%"
		  center>
		  <!-- <span>需要注意的是内容是默认不居中的</span> -->
		  <el-form :model="ruleForm2" status-icon :rules="rules2" ref="ruleForm2" label-width="120px" class="demo-ruleForm reg-content" action="http://127.0.0.1:80/society/welcome/myreg" method="post">
            <el-form-item label="姓名" prop="username">
                <el-input type="text" v-model="ruleForm2.username" auto-complete="off" name="username" :disabled="istransfer||isDemolition"></el-input>
            </el-form-item>
            <el-form-item label="性别" prop="sex">
              <el-radio-group v-model="sex" name="sex" class="radio-sex" :disabled="isDemolition">
                <el-radio label="男"></el-radio>
                <el-radio label="女"></el-radio>
              </el-radio-group>
            </el-form-item> 
            <el-form-item label="年龄" prop="age">
                <el-input type="text" v-model="ruleForm2.age" auto-complete="off" name="age" :disabled="isDemolition"></el-input>
            </el-form-item>
            <!-- <el-form-item label="电子邮箱" prop="email">
                <el-input type="text" v-model="ruleForm2.email" auto-complete="off" name="email"></el-input>
            </el-form-item> -->
            <el-form-item label="手机号码" prop="telephone">
                <el-input type="text" v-model="ruleForm2.telephone" auto-complete="off" name="telephone" :disabled="isDemolition"></el-input>
            </el-form-item>
             <el-form-item label="地址" prop="address">
                <el-input type="text" v-model="ruleForm2.address" auto-complete="off" name="address"></el-input>
            </el-form-item>
            <el-form-item label="在网状态" prop="balance_internet">
              <el-radio-group v-model="balance_internet" name="balance_internet" class="radio-sex" :disabled="isDemolition">
                <el-radio label="入网"></el-radio>
                <el-radio label="退网"></el-radio>
                <el-radio label="拆户"></el-radio>
                <el-radio label="过户"></el-radio>
              </el-radio-group>
            </el-form-item> 
            <el-form-item label="用热状态" prop="balance_state">
              <el-radio-group v-model="balance_state" name="balance_state" class="radio-sex" :disabled="isDemolition">
                <el-radio label="正常"></el-radio>
                <el-radio label="停保"></el-radio>
                <el-radio label="未供热"></el-radio>
              </el-radio-group>
            </el-form-item> 
            <el-form-item label="用热方式" prop="balance_style">
              <el-radio-group v-model="balance_style" name="balance_style" class="radio-sex" :disabled="isDemolition">
                <el-radio label="计量供热"></el-radio>
                <el-radio label="面积供热"></el-radio>
              </el-radio-group>
            </el-form-item> 
            <el-form-item v-if="updateFlag">
                <el-button type="primary" @click="updateForm('ruleForm2')">更新</el-button>
                <el-button @click="resetForm('ruleForm2')">重置</el-button>
            </el-form-item>
            <el-form-item v-else>
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
    created() {
      this.fresh_data();
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
        istransfer: false,//是否过户
        isDemolition: false,//是否拆迁
        updateFlag: false,
        input: '',
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
          telephone: [
            { validator: checkIdTelephone, trigger: 'blur' }
          ],
          address: [
            { validator: checkAddress, trigger: 'blur' }
          ]
        },
        tableData3: [{
          user_id: '1',
          user_name: '张超',
          user_sex: '男',
          user_age: '22',
          user_telephone: '18804502309',
          user_address: '上海市普陀区金沙江路 1518 弄',
          user_entertime: '2016-05-08',
          balance_state: "正常",
          balance_style: "计量供热"
        }, {
          user_id: '2',
          user_name: '刘阿超',
          user_sex: '男',
          user_age: '22',
          user_telephone: '18804502859',
          user_address: '吉林省',
          user_entertime: '2016-05-09',
          balance_state: "正常",
          balance_style: "面积供热"
        }, {
          user_id: '3',
          user_name: 'lisi',
          user_sex: '男',
          user_age: '22',
          user_telephone: '18804502309',
          user_address: '上海市普陀区金沙江路 1518 弄',
          user_entertime: '2016-05-08',
          balance_state: "正常",
          balance_style: "计量供热"
        }, {
          user_id: '5',
          user_name: 'wu',
          user_sex: '男',
          user_age: '22',
          user_telephone: '18804502309',
          user_address: '上海市普陀区金沙江路 1518 弄',
          user_entertime: '2016-05-08',
          balance_state: "正常",
          balance_style: "计量供热"
        }, {
          user_id: '6',
          user_name: 'lalal',
          user_sex: '男',
          user_age: '22',
          user_telephone: '18804502309',
          user_address: '上海市普陀区金沙江路 1518 弄',
          user_entertime: '2016-05-08',
          balance_state: "正常",
          balance_style: "计量供热"
        }, {
          user_id: '7',
          user_name: 'luo',
          user_sex: '男',
          user_age: '22',
          user_telephone: '18804502309',
          user_address: '上海市普陀区金沙江路 1518 弄',
          user_entertime: '2016-05-08',
          balance_state: "正常",
          balance_style: "计量供热"
        }, {
          user_id: '18',
          user_name: 'long',
          user_sex: '男',
          user_age: '22',
          user_telephone: '18804502309',
          user_address: '上海市普陀区金沙江路 1518 弄',
          user_entertime: '2016-05-08',
          balance_state: "正常",
          balance_style: "计量供热"
        }, ],
        multipleSelection: []
      }
    },
    methods: {
      init() {
        this.sex = '';
        this.balance_state = '';
        this.balance_style = '';
        this.balance_internet = '';
        this.ruleForm2.username = ''; 
        this.ruleForm2.age = '';
        this.ruleForm2.telephone =  '';
        this.ruleForm2.address =  ''
      },
      fresh_data() {
          var params = new URLSearchParams(); 
          params.append('username',this.input);
          Axios.post('http://127.0.0.1:80/heatphp/welcome/find_user_information',params).then((res)=>{
            if(res){     
              this.updateFlag = false;
              this.isDemolition = false;
              var _this = this;
              this.tableData3 = res.data || [];
              if(res.data) {
                this.centerDialogVisible = false;
              } else {
                this.centerDialogVisible = true;
              }
            }
        });
      },
      createUser() {
        this.init();
        this.centerDialogVisible = true;
        this.updateFlag = false;    
        this.updateFlag = false;
        this.isDemolition = false; 
      },
      layout: function() {
        localStorage.clear();
        this.$router.push('./login');
      },
      submitForm(formName) {
        this.updateFlag = false;
        this.$refs[formName].validate((valid) => {
          if (valid) {   //获取当前时间
            var params = new URLSearchParams(); 
                params.append('username',this.ruleForm2.username);
                params.append('sex',this.sex);
                params.append('age',this.ruleForm2.age);
                params.append('telephone',this.ruleForm2.telephone); 
                params.append('address',this.ruleForm2.address); 
                params.append('balance_internet',this.balance_internet);
                params.append('balance_state',this.balance_state);
                params.append('balance_style',this.balance_style);
               
                Axios.post('http://127.0.0.1:80/heatphp/welcome/create_user',params).then((res)=>{
                  if(res){
                    this.centerDialogVisible = true;
                    var _this = this;
                    if(res.data) {
                      this.$message({
                        type: 'success',
                        message: '新增用户成功!'
                      });
                      this.centerDialogVisible = false;
                    } else {
                      this.centerDialogVisible = true;
                      this.$message({
                        type: 'warn',
                        message: '新增用户失败!'
                      });
                    }
                  }
              });
          } else {
            return false;
          }
        });
      },
      updateForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            var params = new URLSearchParams(); 
                params.append('username',this.ruleForm2.username);
                params.append('sex',this.sex);
                params.append('age',this.ruleForm2.age);
                params.append('telephone',this.ruleForm2.telephone); 
                params.append('address',this.ruleForm2.address); 
                params.append('balance_internet',this.balance_internet);
                params.append('balance_state',this.balance_state);
                params.append('balance_style',this.balance_style);
               
                Axios.post('http://127.0.0.1:80/heatphp/welcome/update_user',params).then((res)=>{
                  if(res){
                    this.centerDialogVisible = true;
                    var _this = this;
                    if(res.data) {
                      this.fresh_data();
                      this.$message({
                        type: 'success',
                        message: '更新用户成功!'
                      });
                      this.centerDialogVisible = false;
                    } else {
                      this.$message({
                        type: 'warn',
                        message: '更新用户失败!'
                      });
                      this.centerDialogVisible = true;
                    }
                  }
              });
          } else {
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      },
      toggleSelection(rows) {
        if (rows) {
          rows.forEach(row => {
            this.$refs.multipleTable.toggleRowSelection(row,true);
            row.tag ='已收费'
          });
        }
      },
      handleSelectionChange(val) {
        this.multipleSelection = val;
      },
      SendMsg(index, rows) {
         rows = '已收费';
         this.tableData3[index].tag = rows
        },
      Transfer_user(id, name){
        var params = new URLSearchParams(); 
          params.append('username',name);
          Axios.post('http://127.0.0.1:80/heatphp/welcome/find_user_information',params).then((res)=>{
            if(res){     
              this.centerDialogVisible = true;
              this.sex = res.data[0].user_sex;
              this.balance_state = res.data[0].balance_state;
              this.balance_style = res.data[0].balance_style;
              if(res.data[0].balance_internet == 500) {
                this.balance_internet = "入网"
              }
              else if(res.data[0].balance_internet == -500) {
                this.balance_internet = "退网"
              }
              else if(res.data[0].balance_internet == -600) {
                this.balance_internet = "拆户"
              }
              else {
                this.balance_internet = "过户"
              }
              this.ruleForm2.username = res.data[0].user_name;
              this.ruleForm2.age = res.data[0].user_age;
              this.ruleForm2.telephone = res.data[0].user_telephone;
              this.ruleForm2.address = res.data[0].user_address;
              this.updateFlag = true;
              this.istransfer = true;
              this.isDemolition = false;
            }
        });
      },
      Split_user(id, name) {
        var params = new URLSearchParams(); 
          params.append('username',name);
          Axios.post('http://127.0.0.1:80/heatphp/welcome/find_user_information',params).then((res)=>{
            if(res){     
              this.centerDialogVisible = true;
              this.sex = res.data[0].user_sex;
              this.balance_state = res.data[0].balance_state;
              this.balance_style = res.data[0].balance_style;
              if(res.data[0].balance_internet == 500) {
                this.balance_internet = "入网"
              }
              else if(res.data[0].balance_internet == -500) {
                this.balance_internet = "退网"
              }
              else if(res.data[0].balance_internet == -600) {
                this.balance_internet = "拆户"
              }
              else {
                this.balance_internet = "过户"
              }
              this.ruleForm2.username = res.data[0].user_name;
              this.ruleForm2.age = res.data[0].user_age;
              this.ruleForm2.telephone = res.data[0].user_telephone;
              this.ruleForm2.address = res.data[0].user_address;
              this.updateFlag = true;
              this.istransfer = false;
              this.isDemolition = true;
            }
        });
      },
      filterTag(value, row) {
        return row.tag === value;
      },
      select() {
        var params = new URLSearchParams(); 
          params.append('username',this.input);
          Axios.post('http://127.0.0.1:80/heatphp/welcome/find_user_information',params).then((res)=>{
            if(res){     
              this.tableData3 = res.data || [];      
              var _this = this;
             
            }
        });
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
  .input {
    width: 90%;
  }
</style>
