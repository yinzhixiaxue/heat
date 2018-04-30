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
        prop="tag"
        label="状态"
      >
        <template slot-scope="scope">
          <el-tag
            :type="scope.row.tag === '报停' ? 'primary' : 'success'"
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
            @click.native.prevent="SendMsg(scope.$index, scope.row.tag)"
            type="text">
            用热状态变更
          </el-button>
          <el-button type="text" @click="dialogFormVisible = true,isCustomer = true">客户类型变更</el-button>
          <el-button type="text" @click="dialogFormVisible = true,isCustomer = false">计费变更</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-dialog title="变更" :visible.sync="dialogFormVisible">
      <el-form :model="form">
        <div v-if="isCustomer">
          <el-form-item label="客服类型" :label-width="formLabelWidth">
            <el-select v-model="form.CustomerRegion" placeholder="">
              <el-option label="普通" value="普通"></el-option>
              <el-option label="超级" value="超级"></el-option>
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
      Axios.post('http://127.0.0.1:80/heatphp/welcome/arrears_information').then((res) => {
        if (res) {
          var arr = this.tableData3;
          var data = res.data;
          arr.length = data.length;
          for (var i = 0; i < data.length; i++) {
            arr[i].date = data[i].user_entertime;
            arr[i].name = data[i].user_name;
            arr[i].address = data[i].user_address;
            arr[i].tag = '';
          }
          this.tableData3 = arr;
        }

      });
    },
    data() {
      return {
        tableData3: [{
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: '正常'
        }, {
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: '正常'
        }, {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: '正常'
        }, {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: '正常'
        }, {
          date: '2016-05-08',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: '正常'
        }, {
          date: '2016-05-06',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: '正常'
        }, {
          date: '2016-05-07',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: '正常'
        }],
        multipleSelection: [],
        options: [{
          value: '选项1',
          label: '黄金糕'
        }, {
          value: '选项2',
          label: '双皮奶'
        }, {
          value: '选项3',
          label: '蚵仔煎'
        }, {
          value: '选项4',
          label: '龙须面'
        }, {
          value: '选项5',
          label: '北京烤鸭'
        }],
        value4: [],
        gridData: [{
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }],
        isCustomer :false,
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
      handleClick(){
        this.dialogFormVisible = false;
        // 处理是否选择了数据
        this.form.region || this.form.CustomerRegion ? this.popoverThings():''
      },
      popoverThings() {
        const h = this.$createElement;
        this.$msgbox({
          title: '提醒消息',
          message: h('p', null, [
            h('span', null, this.isCustomer?'客户类型为':'计费方式为'),
            h('i', {style: 'color: teal'},this.isCustomer? this.form.CustomerRegion:this.form.region)
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
        ;

      },
      toggleSelection(rows) {
        if (rows) {
          rows.forEach(row => {
            this.$refs.multipleTable.toggleRowSelection(row, true);
            row.tag = '已批量发送催缴短信'
          });
        }
      },
      handleSelectionChange(val) {
        this.multipleSelection = val;
      },
      SendMsg(index, rows) {
        rows = '报停';
        this.tableData3[index].tag = rows
      },
      filterTag(value, row) {
        return row.tag === value;
      },
    }
  }
</script>
