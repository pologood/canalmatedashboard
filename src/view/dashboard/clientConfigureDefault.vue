<template>
  <div class="delay-content">
    <div class="table">
      <el-table :data="serverConfigureList.slice((currentPage-1)*pagesize,currentPage*pagesize)" highlight-current-row border stripe>
        <el-table-column type="selection" width="55" align="center">
        </el-table-column>
        <el-table-column prop="Id" label="server_id" align="center" sortable>
        </el-table-column>
        <el-table-column prop="Ip" label="server_ip" align="center" sortable>
        </el-table-column>
        <el-table-column prop="Port" label="server端口" align="center" sortable>
        </el-table-column>
        <el-table-column prop="zkServers" label="zkServers" align="center" sortable>
        </el-table-column>
        <el-table-column prop="destinations" label="destinations" align="center" sortable>
        </el-table-column>
        <el-table-column label="操作" fixed="right" align="center" width="150">
          <template scope="scope">
            <el-button size="small" @click="handleUpdate(scope.row)">修改</el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-col :span="24" class="toolbar">
        <!--<el-button type="danger" @click="batchRemove" :disabled="this.sels.length===0">批量删除</el-button>-->
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="currentPage"
          :page-sizes="[15, 30, 45, 60]"
          :page-size="pagesize"
          layout="total, sizes, prev, pager, next, jumper"
          :total="totalCount"
        >
        </el-pagination>
      </el-col>
    </div>
    <el-dialog
      title="修改信息"
      :visible.sync="editDialogVisible"
      width="30%"
      center>
      <el-form :model="editForm" label-width="120px" :rules="editFormRules" ref="editForm">
        <el-form-item label="canal_id" prop="userPassword">
          <el-input v-model="editForm.userPassword" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="canal_ip" prop="userMobile">
          <el-input v-model="editForm.userMobile" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="canal_poru" prop="userEmail">
          <el-input v-model="editForm.userEmail" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="canal_zkServers" prop="superior">
          <el-input v-model="editForm.superior" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="canal_destinations" prop="superior">
          <el-input v-model="editForm.superior" auto-complete="off"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
         <el-button @click="editDialogVisible = false">取 消</el-button>
         <el-button type="primary" @click.native="updateSubmit">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>
<script>
  import axios from "axios"
  export default {
    data(){
      return{
        serverConfigureList:[],
        totalCount:0,
        currentPage:1,
        pagesize:15,
        editDialogVisible:false,
        addDialogVisible:false,
        //修改界面
        editForm: {
          pkUserId: 0,
          userCode: "",
          userName: "",
          superior: "",
          userMobile: "",
          userEmail: "",
          createUser: "",
          insertTime: "",
          userPassword: "",
          updateTime: "",
          updateUser: ""
        },
      }
    },
    mounted(){
      this.getList()
    },
    methods:{
      //获取列表
      getList(){
        axios.get('../../../static/serverList.json').then((response) => {
          let list = response.data.list;
          this.serverConfigureList = list;
          this.totalCount = list.length;
          console.log(list)
        })
      },
      //删除列表
      handleDel(row) {
        //console.log(row.pkUserId)
        this.$confirm('确认删除该条记录吗?', '提示', {
          //confirmButtonText: '确定',
          // cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });
        });
      },
      //显示修改界面
      handleUpdate(row) {
        this.editDialogVisible = true;
        this.editForm = Object.assign({}, row)
      },
      //修改提交
      updateSubmit(){
        this.$refs.editForm.validate((valid) => {
          if (valid) {
            this.$confirm('确认提交吗？', '提示', {}).then(() => {
              let para = Object.assign({}, this.editForm);
              console.log(para)
              //console.log(para.userPassword)
            })
          }
        })
      },
      handleSizeChange(val){
        this.pagesize = val
      },
      handleCurrentChange(val){
        this.currentPage = val
      },
    }
  }
</script>
<style>
  .el-table td, .el-table th{
    padding:5px 0;
  }
  .el-pagination{
    text-align: right;
    margin-top:15px;
  }
</style>
