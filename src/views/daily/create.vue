<template>
  <div class="app-container">
    <h2 align="center">测试日报</h2>
    <h3>一、项目进展</h3>
    <el-row>
        <el-button type="primary" size="mini" @click="pro_drawer=true">新增</el-button>
    </el-row>
    <el-table :data="projectTableData" stripe style="width: 100%">
        <el-table-column type="index" label="序号"></el-table-column>
        <el-table-column prop="name" label="项目名称" ></el-table-column>
        <el-table-column prop="priority" label="优先级" ></el-table-column>
        <el-table-column prop="progress" label="进度" ></el-table-column>
        <el-table-column prop="period" label="测试日期" ></el-table-column>
        <el-table-column prop="manpower" label="投入人力" ></el-table-column>
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

    <el-drawer
        title="添加项目"
        :visible.sync="pro_drawer"
        :with-header="false"
        size="40%">
        <el-form ref="form" :model="form" label-width="120px">
            <el-form-item label="项目名称：">
                <el-input v-model="form.name" size="small" clearable type="text" placeholder="请输入项目名称" style="width:200px" />
            </el-form-item>
            <el-form-item label="优先级：">
                <el-select v-model="form.priority" size="small" placeholder="请选择优先级">
                    <el-option label="P0" value="1" />
                    <el-option label="P1" value="2" />
                    <el-option label="P2" value="3" />
                    <el-option label="P3" value="4" />
                </el-select>
            </el-form-item>
            <el-form-item label="进度(%)：">
                <el-input-number v-model="form.progress" size="small" controls-position="right"  :min="5" :max="100"></el-input-number>
            </el-form-item>
            <el-form-item label="测试日期：">
                <el-date-picker
                    v-model="form.period"
                    type="daterange"
                    range-separator="至"
                    value-format="yyyy-MM-dd"
                    start-placeholder="开始日期"
                    end-placeholder="结束日期" size="small">
                </el-date-picker>
            </el-form-item>
            <el-form-item label="投入人力：">
                <el-input v-model="form.manpower" size="small" clearable type="text" placeholder="请输入测试人员" style="width:200px" />
                <el-input-number v-model="form.hours" size="small" controls-position="right"  :min="1" :max="8" step="0.5"></el-input-number>
            </el-form-item>
            <el-form-item>
                <el-button size="small"  round @click="pro_drawer=false">取消</el-button>
                <el-button type="primary" size="small"  round @click="submit">提交</el-button>
            </el-form-item>
        </el-form>
    </el-drawer>

    <h3>二、问题&风险</h3>
    <el-table :data="dangerTableData" stripe style="width: 100%">
        <el-table-column type="index" label="序号" ></el-table-column>
        <el-table-column prop="description" label="问题描述" ></el-table-column>
        <el-table-column prop="edit_time" label="编辑日期" width="200px"></el-table-column>
        <el-table-column prop="operate" label="操作" width="100px"></el-table-column>
    </el-table>
    <h3>三、重点事项</h3>
    <el-table :data="importTableData" stripe style="width: 100%">
        <el-table-column type="index" label="序号" ></el-table-column>
        <el-table-column prop="description" label="问题描述" ></el-table-column>
        <el-table-column prop="edit_time" label="编辑日期" width="200px"></el-table-column>
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
        busi_line: 'sss',
        form:{
            name: "测试项目名",
            busi_line: "业务线",
            priority: "1",
            progress: 10,
            manpower: "",
            period: [],
            hours: 1,
            item_type: 1
        },
        pro_drawer: false,
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
      submit(){
          this.form.period = this.form.period.join("至")
          this.req({
          url: "/daily/add",
          data: this.form,
          method: "POST"
        }).then(
          res => {
            console.log("res==== :", res);
          },
          err => {
            console.log("err :", err);
          }
        );
      }
    }
  }
</script>