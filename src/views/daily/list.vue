<template>
  <div class="app-container">
    <el-row>
      <el-col :span="2">
          <el-input v-model="busi_line" type="text" prefix-icon="el-icon-search"
          placeholder="输入业务线" clearable style="width:150px" />
      </el-col>
      <el-col :span="1">
        <span>&nbsp;</span>
      </el-col>
      <el-col :span="2">
        <el-button type="primary" @click="select">查询</el-button>
      </el-col>
      <el-col :span="2">
        <el-button type="primary" @click="create">新增</el-button>
      </el-col>
    </el-row>

    <el-divider></el-divider>
    <el-table
      :data="tableData"
      stripe border 
      :header-cell-style="{backgroundColor:'#C2BFC3', color: '#000102'}"
      max-height="800"
      size="middle"
      fit
      style="width: 98%;margin-left: 1%; ">
      <el-table-column
        prop="id"
        label="ID"
        width="70">
      </el-table-column>
      <el-table-column
        prop="date"
        label="日期"
        align="center">
      </el-table-column>
      <el-table-column
        prop="busi_line"
        label="业务线"
        align="center">
      </el-table-column>
      <el-table-column
        prop="status"
        label="状态"
        align="center">
      </el-table-column>
      <el-table-column
        prop="author"
        label="更新人">
      </el-table-column>
      <el-table-column
        prop="create_time"
        label="创建时间">
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
        tableData: [],
        busi_line: ''
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