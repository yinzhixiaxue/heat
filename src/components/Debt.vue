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
            :type="scope.row.tag === '已退费' ? 'primary' : 'success'"
            close-transition>{{scope.row.tag}}</el-tag>
        </template>
      </el-table-column>
      <el-table-column
        fixed="right"
        label="操作"
        width="120">
        <template slot-scope="scope">
          <el-button
            @click.native.prevent="SendMsg(scope.$index, scope.row.tag)"
            type="text"
            size="small">
            催缴费用
          </el-button>
          <!-- <el-button type="text" size="small" @click.native.prevent="BackCharge(scope.$index, scope.row.tag)">退费</el-button> -->
        </template>
      </el-table-column>
    </el-table>
    <div style="margin-top: 20px">
      <el-button @click="toggleSelection(tableData3)">批量发送催缴短信</el-button>
    </div>
  </div>

</template>

<script>
import Axios from 'axios';
  export default {
    created() {
          Axios.post('http://127.0.0.1:80/heatphp/welcome/arrears_information').then((res)=>{
            if(res){     
              var arr = this.tableData3;
              var data = res.data;
              arr.length = data.length;
               for(var i = 0;i < data.length; i++) {
                    arr[i].date = data[i].user_entertime;
                    arr[i].name = data[i].user_name;
                    arr[i].address = data[i].user_address;
                    arr[i].tag = '';
                  }
                  this.tableData3 = arr;
              // for(var i = 0;i < data.length; i++) {
              //   arr[i].date = data[i].user_entertime;
              //   arr[i].name = data[i].user_name;
              //   arr[i].address = data[i].user_address;
              //   arr[i].tag = '';
              // }
              // this.tableData3 = arr;
            }
              
        });
    }, 
    data() {
      return {
        tableData3: [{
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: ''
        }, {
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: ''
        }, {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: ''
        }, {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: ''
        }, {
          date: '2016-05-08',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: ''
        }, {
          date: '2016-05-06',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: ''
        }, {
          date: '2016-05-07',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          tag: ''
        }],
        multipleSelection: []
      }
    },

    methods: {
      toggleSelection(rows) {
        if (rows) {
          rows.forEach(row => {
            this.$refs.multipleTable.toggleRowSelection(row,true);
            row.tag ='已批量发送催缴短信'
          });
        }
      },
      handleSelectionChange(val) {
        this.multipleSelection = val;
      },
      SendMsg(index, rows) {
         rows = '已发送催缴短信';
         this.tableData3[index].tag = rows
        },
      // BackCharge(index, rows){
      //   rows = '已退费';
      //   this.tableData3[index].tag = rows
      // },
      filterTag(value, row) {
        return row.tag === value;
      },
    }
  }
</script>
