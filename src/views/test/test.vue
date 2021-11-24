<template>
  <div class="app-container">
    <h2 align="center">测试日报</h2>
    <h3>一、项目进展</h3>
    <el-row>
        <el-button type="primary" size="mini" @click="handleAdd()">新增</el-button>
    </el-row>
    <el-table :data="projectTableData" stripe style="width: 100%">
        <el-table-column type="index" label="序号"></el-table-column>
        <el-table-column prop="name" label="项目名称" >
            <template slot-scope="scope">
                <el-input size="mini" v-model="scope.row.name" v-if="scope.row.flag" @blur="inputClick(scope.row)" v-focus></el-input>
                <!-- <span v-if="!scope.row.flag">{{scope.row.factorLevel}}</span> -->
            </template>
        </el-table-column>
        <el-table-column prop="priority" label="优先级" ></el-table-column>
        <el-table-column prop="progress" label="进度" ></el-table-column>
        <el-table-column prop="date" label="测试日期" ></el-table-column>
        <el-table-column prop="qa" label="投入人力" ></el-table-column>
        <el-table-column prop="hours" label="投入时长" ></el-table-column>
        <el-table-column prop="edit_time" label="编辑日期" ></el-table-column>
        <el-table-column prop="operate" label="操作" width="100px">
            <template slot-scope="scope">
                <div style="display: flex;justify-content: space-between;">
                    <el-link icon="el-icon-delete" size="middle" @click="handleDelete(scope.$index, scope.row)"
                        type="danger" :underline="false"></el-link>
                </div>
            </template>
        </el-table-column>
    </el-table>

    <h3>二、问题&风险</h3>
    <el-table :data="dangerTableData" stripe style="width: 100%">
        <el-table-column type="index" label="序号" ></el-table-column>
        <el-table-column prop="name" label="项目名称" ></el-table-column>
        <el-table-column prop="priority" label="优先级" ></el-table-column>
        <el-table-column prop="progress" label="进度" ></el-table-column>
        <el-table-column prop="date" label="测试日期" ></el-table-column>
        <el-table-column prop="qa" label="投入人力" ></el-table-column>
        <el-table-column prop="hours" label="投入时长" ></el-table-column>
        <el-table-column prop="edit_time" label="编辑日期" ></el-table-column>
        <el-table-column prop="operate" label="操作" width="100px"></el-table-column>
    </el-table>
    <h3>三、重点事项</h3>
    <el-table :data="importTableData" stripe style="width: 100%">
        <el-table-column type="index" label="序号" ></el-table-column>
        <el-table-column prop="description" label="问题描述" ></el-table-column>
        <el-table-column prop="operate" label="操作" width="100px" align="center">
            <template slot-scope="scope">
                <div style="display: flex;justify-content: space-between;">
                    <el-link icon="el-icon-plus" size="mini" type="primary"
                        @click="handleAdd(scope.$index, scope.row)" :underline="false"
                        style="font-size: 14px;margin-left: 40px"></el-link>
                    <el-link icon="el-icon-delete" size="mini" @click="handleDelete(scope.$index, scope.row)"
                        type="danger" :underline="false"></el-link>
                </div>
            </template>
        </el-table-column>
    </el-table>
  </div>
  
</template>

<script>
import Vue from "vue";
import axios from "axios";
Vue.prototype.$axios = axios;
  export default {
    data() {
      return {
        projectTableData: [],
        dangerTableData: [],
        importTableData: [],
        busi_line: 'sss'
      }
    },
    created(){
      this.getList()
    },
    methods: {
      filterHandler(value, row, column) {
          const property = column['property'];
          return row[property] === value;
      },
      handleAdd() {
        // if ((row.date && row.name && row.addr) || (row.date && row.addr)) {
        //     this.tableData.push({
        //         date: '',
        //         name: '',
        //         addr: ''
        //     });
        // }
        this.projectTableData.push({
            name: 'test',
            priority: 'P0',
            progress: '90%',
            date: '2021-09-09',
            qa: 'hjk',
            hours: '3h',
            edit_time: '2021-10-09',
        });
    },
    handleDelete(index, row) {
        // debugger
        if (index > 0) {
            this.projectTableData.splice(index, 1);
        }
    },
    inputClick(row){
        row.flag=false
    },
      select(){
        this.getList()
      },
      create(){
        this.$router.push({ path: "/daily/create" });
      },
      getList(){
        let that = this;
        this.req({
          url: `/daily/list?page=1&limit=20&busi_line=${that.busi_line}`,
          data: {},
          method: "GET"
        }).then(
          res => {
            console.log("res==== :", res);
            that.tableData = res.data;
          },
          err => {
            console.log("err :", err);
          }
        );
      }
    }
  }
</script>