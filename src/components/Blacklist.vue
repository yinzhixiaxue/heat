<template>
	<div>
     <el-table
      ref="multipleTable"
      :data="tableData3"
      tooltip-effect="dark"
      style="width: 100%"
      @selection-change="handleSelectionChange">
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
        prop="balance_time"
        label="交费时间">
      </el-table-column>
    </el-table>
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
      return {
        updateFlag: false,
        input: '',
        name: '你好',
        centerDialogVisible: false,
        tip: '',
        sex: '',
        balance_state: '',
        balance_style: '',
        balance_internet: '',
        tableData3: [{
          user_id: '1',
          user_name: '张超',
          user_sex: '男',
          user_age: '22',
          user_telephone: '18804502309',
          user_address: '上海市普陀区金沙江路 1518 弄',
          user_entertime: '2016-05-08',
          balance_state: "正常",
          balance_style: "计量供热",
          balance_time: '2016-05-08'
        }, {
          user_id: '2',
          user_name: '刘阿超',
          user_sex: '男',
          user_age: '22',
          user_telephone: '18804502859',
          user_address: '吉林省',
          user_entertime: '2016-05-09',
          balance_state: "正常",
          balance_style: "面积供热",
          balance_time: '2016-05-08'

        }, {
          user_id: '3',
          user_name: 'lisi',
          user_sex: '男',
          user_age: '22',
          user_telephone: '18804502309',
          user_address: '上海市普陀区金沙江路 1518 弄',
          user_entertime: '2016-05-08',
          balance_state: "正常",
          balance_style: "计量供热",
          balance_time: '2016-05-08'
        }, {
          user_id: '5',
          user_name: 'wu',
          user_sex: '男',
          user_age: '22',
          user_telephone: '18804502309',
          user_address: '上海市普陀区金沙江路 1518 弄',
          user_entertime: '2016-05-08',
          balance_state: "正常",
          balance_style: "计量供热",
          balance_time: '2016-05-08'
        }, {
          user_id: '6',
          user_name: 'lalal',
          user_sex: '男',
          user_age: '22',
          user_telephone: '18804502309',
          user_address: '上海市普陀区金沙江路 1518 弄',
          user_entertime: '2016-05-08',
          balance_state: "正常",
          balance_style: "计量供热",
          balance_time: '2016-05-08'
        }, {
          user_id: '7',
          user_name: 'luo',
          user_sex: '男',
          user_age: '22',
          user_telephone: '18804502309',
          user_address: '上海市普陀区金沙江路 1518 弄',
          user_entertime: '2016-05-08',
          balance_state: "正常",
          balance_style: "计量供热",
          balance_time: '2016-05-08'
        }, {
          user_id: '18',
          user_name: 'long',
          user_sex: '男',
          user_age: '22',
          user_telephone: '18804502309',
          user_address: '上海市普陀区金沙江路 1518 弄',
          user_entertime: '2016-05-08',
          balance_state: "正常",
          balance_style: "计量供热",
          balance_time: '2016-05-08'
        }, ],
        multipleSelection: []
      }
    },
    methods: {
      fresh_data() {
          var params = new URLSearchParams(); 
          params.append('username',this.input);
          Axios.post('http://127.0.0.1:80/heatphp/welcome/find_blacklist',params).then((res)=>{
            if(res){           
              var _this = this;
              this.tableData3 = res.data || [];
              if(res.data) {
                console.log(res.data);
                
              } else {
                
              }
            }
        });
      },
      handleSelect(key, keyPath) {
        console.log(key, keyPath);
      },
      layout: function() {
        localStorage.clear();
        this.$router.push('./login');
      },


      handleSelectionChange(val) {
        this.multipleSelection = val;
      },
      filterTag(value, row) {
        return row.tag === value;
      },
      select() {
        var params = new URLSearchParams(); 
          params.append('username',this.input);
          Axios.post('http://127.0.0.1:80/heatphp/welcome/find_blacklist',params).then((res)=>{
            if(res){     
            	console.log(res.data);
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

