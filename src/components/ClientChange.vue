<template>
  <div>
    <el-table
      ref="multipleTable"
      :data="tableData3"
      tooltip-effect="dark"
      style="width: 100%"
      @selection-change="handleSelectionChange">
      <el-table-column
        type="selection"
        width="55">
      </el-table-column>
      <el-table-column
        label="日期"
        width="120">
        <template slot-scope="scope">{{ scope.row.date }}</template>
      </el-table-column>
      <el-table-column
        prop="name"
        label="姓名"
        width="120">
      </el-table-column>
      <el-table-column
        prop="address"
        label="地址"
        show-overflow-tooltip>
      </el-table-column>
      <el-table-column
        prop="heatingMode"
        label="供热方式"
        width="120">
        <template slot-scope="scope">
          <el-tag
            :type="scope.row.heatingMode === '计量供热' ? 'primary' : 'success'"
            close-transition>{{scope.row.heatingMode}}
          </el-tag>
        </template>
      </el-table-column>
      <el-table-column
        prop="tag"
        label="用热状态"
      >
        <template slot-scope="scope">
          <el-tag
            :type="scope.row.tag === '正常' ? 'primary' : 'success'"
            close-transition>{{scope.row.tag}}
          </el-tag>
        </template>
      </el-table-column>
      <el-table-column
        fixed="right"
        label="操作"
        width="120">
        <template slot-scope="scope">
          <el-button
            @click.native.prevent=" index = scope.$index,dialogFormVisible = true,isCustomer = true"
            type="text">
            客户类型变更
          </el-button>
          <!--<el-button type="text" @click="dialogFormVisible = true,isCustomer = true">客户类型变更</el-button>-->
          <el-button type="text" @click=" index = scope.$index,dialogFormVisible = true,isCustomer = false">计费变更</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-dialog title="变更" :visible.sync="dialogFormVisible">
      <el-form :model="form">
        <div v-if="isCustomer">
          <el-form-item label="用热类型" :label-width="formLabelWidth">
            <el-select v-model="form.CustomerRegion" placeholder="">
              <el-option label="正常" value="正常"></el-option>
              <el-option label="停保" value="停保"></el-option>
              <el-option label="未供热" value="未供热"></el-option>
            </el-select>
          </el-form-item>
        </div>
        <div v-else>
          <el-form-item label="用热方式" :label-width="formLabelWidth">
            <el-select v-model="form.region" placeholder="">
              <el-option label="计量供热" value="计量供热"></el-option>
              <el-option label="面积供热" value="面积供热"></el-option>
            </el-select>
          </el-form-item>
        </div>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="handleClick">确 定</el-button>
      </div>
    </el-dialog>
  </div>

</template>

<script>
  import Axios from 'axios';

  export default {
    created() {
      this.find_information();
    },
    data() {
      return {
        tableData3: [{
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: '正常',
          heatingMode : '计量供热'
        }, {
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: '正常',
          heatingMode : '计量供热'
        }, {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: '正常',
          heatingMode : '计量供热'
        }, {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: '正常',
          heatingMode : '计量供热'
        }, {
          date: '2016-05-08',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: '正常',
          heatingMode : '计量供热'
        }, {
          date: '2016-05-06',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: '正常',
          heatingMode : '计量供热'
        }, {
          date: '2016-05-07',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: '正常',
          heatingMode : '计量供热'
        }, {
          date: '2016-05-07',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: '正常',
          heatingMode : '计量供热'
        }, {
          date: '2016-05-07',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: '正常',
          heatingMode : '计量供热'
        }, {
          date: '2016-05-07',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: '正常',
          heatingMode : '计量供热'
        }, {
          date: '2016-05-07',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: '正常',
          heatingMode : '计量供热'
        }],
        index : '', // 保存每行的index值为下面改变状态使用
        multipleSelection: [],
        isCustomer: false,
        dialogTableVisible: false,
        dialogFormVisible: false,
        form: {
          region: '',
          delivery: false,
          CustomerRegion: ''
        },
        formLabelWidth: '120px'
      };
    },

    methods: {
      find_information() {
        Axios.post('http://127.0.0.1:80/heatphp/welcome/find_user_information').then((res) => {
        if (res) {
          var arr = this.tableData3;
          var data = res.data;
          arr.length = data.length;
          for (var i = 0; i < data.length; i++) {
            arr[i].date = data[i].user_entertime;
            arr[i].name = data[i].user_name;
            arr[i].address = data[i].user_address;
            arr[i].tag = data[i].balance_state;
            arr[i].heatingMode = data[i].balance_style;
          }
          this.tableData3 = arr;
        }
      });
      },
      handleClick() {
        this.dialogFormVisible = false;
        if(this.isCustomer){
          this.changeState(this.index);
        }else {
          this.changeStyle(this.index);
        }
        this.form.region || this.form.CustomerRegion ? this.popoverThings() : ''
      },
       // 第二个弹出层代码
      popoverThings() {
        const h = this.$createElement;
        this.$msgbox({
          title: '提醒消息',
          message: h('p', null, [
            h('span', null, this.isCustomer ? '用热状态' : '计费方式为'),
            h('i', {style: 'color: teal'}, this.isCustomer ? this.form.CustomerRegion : this.form.region)
          ]),
          showCancelButton: true,
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          beforeClose: (action, instance, done) => {
            if (action === 'confirm') {
              instance.confirmButtonLoading = true;
              instance.confirmButtonText = '执行中...';
              setTimeout(() => {
                done();
                instance.confirmButtonLoading = false;
              }, 0);
            } else {
              done();
            }
          }
        }).then(() => {
          this.$message({
            type: 'success',
            message: '变更成功!'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '取消变更'
          });
        });
      },
      handleSelectionChange(val) {
        this.multipleSelection = val;
      },
      changeState(index) {
        var params = new URLSearchParams(); 
          params.append('username',this.tableData3[index].name);
          params.append('balance_state',this.form.CustomerRegion);
          Axios.post('http://127.0.0.1:80/heatphp/welcome/update_balance_state',params).then((res)=>{
            if(res){           
              if(res.data) {
                setTimeout(()=>{
                  let rows = this.form.CustomerRegion;
                  this.tableData3[index].tag = rows ;
                  this.form.CustomerRegion = '';  // 清空选择框的数据。
                },3000)
               
              } else {
                alert('fail');
              }
            }
        });
      },
      changeStyle(index){
        var params = new URLSearchParams(); 
          params.append('username',this.tableData3[index].name);
          params.append('balance_style',this.form.region);
          Axios.post('http://127.0.0.1:80/heatphp/welcome/update_balance_style',params).then((res)=>{
            if(res){           
              if(res.data) {
               setTimeout(()=>{
                let rows = this.form.region;
                this.tableData3[index].heatingMode = rows ;
              },3000)
              } else {
                alert('fail');
              }
            }
        });
      },
      filterTag(value, row) {
        return row.tag === value;
      },
    }
  }
</script>
