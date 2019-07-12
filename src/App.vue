<template>
  <div id="app">
    <el-form :inline="true" :model="formInline" class="demo-form-inline">
      <el-form-item label="网站名称">
        <el-input v-model="formInline.website_name" placeholder="网站名称"></el-input>
      </el-form-item>
      <el-form-item label="网址">
        <el-input v-model="formInline.website_address" placeholder="网址"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
      </el-form-item>
    </el-form>

    <el-table
            :data="tableData"
            style="width: 100%">
      <el-table-column
              label="网站名称"
              width="180">
        <template slot-scope="scope">
          <i class="el-icon-time"></i>
          <span style="margin-left: 10px">{{ scope.row.website_name }}</span>
        </template>
      </el-table-column>
      <el-table-column
              label="网址"
              width="180">
        <template slot-scope="scope">
          <span style="margin-left: 10px">{{ scope.row.website_address }}</span>
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button
                  size="mini"
                  @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
          <el-button
                  size="mini"
                  type="danger"
                  @click="handleDelete(scope.$index, scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>

</template>

<script>
  import axios from 'axios'
  export default {
    data() {
      return {
        formInline: {
          website_address: '',
          website_name: ''
        },
        tableData: [{
          website_address: 'www.baidu.com',
          website_name: 'baidu'
        },{
          website_address: 'www.baidu.com',
          website_name: 'baidu'
        }]
      }
    },
    mounted(){
      this.updataTableData()
    },
    methods: {
      handleEdit(index, row) {
        console.log(index, row);
      },
      handleDelete(index, row) {
        console.log(index, row);
      },
      onSubmit() {
        axios.post('http://localhost:8000/polls/add_data',{
          "website_name":this.formInline.website_name,
          "website_address":this.formInline.website_address
        }).then(res=>{
          this.updataTableData()
        })
      },
      updataTableData(){
        axios.get('http://localhost:8000/polls/get_data').then(res=>{
          let data = res.data.split('\n')
          let arr = []
          for (let i=0;i<data.length;i++){
            arr.push(JSON.parse(data[i]))
          }
          this.tableData = arr
        })
      }
    }
  }
</script>