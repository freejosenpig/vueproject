<template>
  <!-- 主内容 -->
  <div class="userlist">
    <!-- 标题 -->
    <div class="page-tag">
      <span style="float: left">护理项目管理</span>
      <!-- 标签页 -->
    </div>
    <!-- 表体内容 -->
    <div class="userlist-mian">
       <!-- 修改 -->
      <el-dialog title="项目修改" v-model="dialogFormVisible" width="35%" :before-close="close">
        <el-form :model="add">
        <el-form-item label="项目名称" prop="planName">
            <el-input v-model.trim="add.planName" placeholder="请填写" style="width: 250px" />  
          </el-form-item>
          <el-form-item label="护理类型：" prop="ptype">
           <el-input v-model.trim="add.ptype" placeholder="请填写" style="width: 250px" />  
          </el-form-item>
          <el-form-item label="护理方式：" prop="pway">
            <el-input v-model.trim="add.pway" placeholder="请填写" style="width: 250px" /> 
          </el-form-item>
		      <el-form-item label="项目费用：" prop="pmonry">
            <el-input v-model.trim="add.pmonry" style="width: 250px" placeholder="请填写" />
          </el-form-item>
          <el-form-item label="所需时间：" prop="ptime">
            <el-input v-model.trim="add.ptime" style="width: 250px" placeholder="请填" />
          </el-form-item>
          <el-form-item label="项目介绍：">
            <el-input
              type="textarea"
              :rows="3"
              placeholder="请填写"
              v-model.trim="add.pintroduce"
            >
            </el-input>
          </el-form-item>  
        </el-form>
        <template #footer>
          <span class="dialog-footer">
            <el-button @click="dialogFormVisible = false">取 消</el-button>
            <el-button type="primary" @click="update"
              >确 定</el-button
            >
          </span>
        </template>
      </el-dialog> 

      <!-- 新增 -->
      <el-dialog title="新增项目" v-model="dialogaddVisible" width="35%">
        <el-form :model="add"  ref="add">
          <el-form-item label="项目名称" prop="planName">
            <el-input v-model.trim="add.planName" placeholder="请填写" style="width: 250px" />  
          </el-form-item>
          <el-form-item label="护理类型：" prop="ptype">
           <el-input v-model.trim="add.ptype" placeholder="请填写" style="width: 250px" />  
          </el-form-item>
          <el-form-item label="护理方式：" prop="pway">
            <el-input v-model.trim="add.pway" placeholder="请填写" style="width: 250px" /> 
          </el-form-item>
		      <el-form-item label="项目费用：" prop="pmonry">
            <el-input v-model.trim="add.pmonry" style="width: 250px" placeholder="请填写" />
          </el-form-item>
          <el-form-item label="所需时间：" prop="ptime">
            <el-input v-model.trim="add.ptime" style="width: 250px" placeholder="请填" />
          </el-form-item>
          <el-form-item label="项目介绍：">
            <el-input
              type="textarea"
              :rows="3"
              placeholder="请填写注意事项"
              v-model.trim="add.pintroduce"
            >
            </el-input>
          </el-form-item>  
        </el-form>
        <template #footer>
          <span class="dialog-footer">
            <el-button @click="dialogaddVisible = false">取 消</el-button>
            <el-button type="primary" @click="addnewok('add')">确 定</el-button>
          </span>
        </template>
      </el-dialog>

      <div style="padding: 10px 25px">
        <el-button
          icon="el-icon-plus"
          type="primary"
          size="large"
          @click="addnew()"
          >新增登记</el-button
        >
        <!-- 模糊查询 -->
        <div style="float: right">
            项目名称：
          <el-input
            clearable
            v-model="vagueorderid"
            placeholder="请输入"
            style="width: 200px"
            size="large"
          />
          
          <el-button icon="el-icon-search" size="large" @click="join()" 
            >查询</el-button
          >
        </div>
      </div>
      <el-table :data="tableData" style="width: 100%; margin-bottom: 20px">
        <el-table-column prop="id" label="项目编号" />
        <el-table-column prop="planName" label="项目名称" />
        <el-table-column prop="ptype" label="护理类型" />
        <el-table-column prop="pway" label="护理方式"/>
        <el-table-column prop="pmonry" label="项目费用" width="200px" />
        <el-table-column prop="ptime" label="所需时间" width="200px" />
        <el-table-column prop="pintroduce" label="项目介绍" />
        <el-table-column prop="cz" label="操作" width="250px">
          <template #default="scope">
            <el-button
              type="text"
              class="el-icon-edit"
              size="samll"
              @click="updateconshow(scope.row)"
              >修改</el-button
            >
            <el-button
              type="text"
              class="el-icon-delete"
              size="samll"
              @click="delet(scope.$index)"
              v-if="tableData[scope.$index].userId != 1"
              >删除</el-button
            >    
          </template>
        </el-table-column>
      </el-table>
    </div>
    <!-- 表尾分页显示 -->
    <div class="userlist-footer">
      <el-pagination
        background
        layout="prev, pager, next"
        :total="max"
        :page-size="pagesize"
        style="margin-top: 50px"
        @current-change="handleCurrentChange"
        v-model:currentPage="currentPage"
      >
      </el-pagination>
    </div>
  </div>
</template>

<script>
import { ElMessage } from 'element-plus'
export default {
  data() {
    return {
      //表单数据
      tableData: [],
      all: [],
      dialogFormVisible: false,
      dialogFormVisible1: false,
      dialogaddVisible: false,
	  
      form: {
        id: '',
        planName: '',
        ptype: '',
        pway: '',
        pmonry: '',
        ptime: '',
        pintroduce: '',
      },
      add: {
        id: '',
        planName: '',
        ptype: '',
        pway: '',
        pmonry: '',
        ptime: '',
        pintroduce: '',
      },
      options: [],
      condition: { planName: ''}, //查询条件
      thisusername: '',
      thisuserphone: '',
      vagueorderid: '',
      vagueorderid1: '',
      vagueorderid2: '',
      //分页
      pagesize: 10,
      max: 0,
      currentPage: 1,
    }
  },
  methods: {
    //模糊查询
    join() {
      this.condition.planName = this.vagueorderid
      this.findpage()
    },
    //新增
    addnew() {
      this.add = {
        id: '',
        planName: '',
        ptype: '',
        pway: '',
        pmonry: '',
        ptime: '',
        pintroduce: '',
      }
      this.dialogaddVisible = true
    },
    //确认添加
    addnewok(formName) {
	  var _this = this
		this.add.createPeople = this.$store.state.userInfo.userName
		this.axios.post("http://localhost:8188/NurseProject/addNurse",this.add)      
        .then(function(response) {
         _this.dialogaddVisible = false;
						console.log(response)
							ElMessage.success({
                    		message: '用户信息添加成功',
                    		type: 'success',
                  			})
                  _this.findpage()
						_this.axios.get("http://localhost:8188/NurseProject/selectAll").then(function(response) {
							console.log(response)
							_this.tableData = response.data.data			
						}).catch(function(error) {
							console.log(error)
							ElMessage.warning({
                    		message: '用户信息添加失败',
                    		type: 'warning',
                  		})
						})
					}).catch(function(error) {
						console.log(error)
					})
    },
    //修改显示
    updateconshow(row) {
				this.add.id = row.id
                this.add.planName = row.planName
                this.add.ptype = row.ptype
                this.add.pway = row.pway
                this.add.pmonry = row.pmonry
                this.add.ptime = row.ptime
                this.add.pintroduce = row.pintroduce
				this.dialogFormVisible = true
			},
    //修改
    update() {
      var _this = this
      console.log(_this.form)
      this.axios.post("http://localhost:8188/NurseProject/updateNurse", this.add)
        .then(function (response) {
         console.log(response)
            	ElMessage.success({
                    		message: '用户信息添加成功',
                    		type: 'success',
                  			})
						_this.dialogFormVisible = false
						_this.axios.get("http://localhost:8188/NurseProject/selectAll", {
						}).then(function(response) {
							console.log(response)
							_this.tableData = response.data.list
						}).catch(function(error) {
							console.log(error)
						})
					}).catch(function(error) {
						console.log(error)
            ElMessage.warning({
                    		message: '用户信息添加失败',
                    		type: 'warning',
                  		})
					})
    },
    //删除
    delet(index) {
      this.form = {
        id: '',
        planName: '',
        ptype: '',
        pway: '',
        pmonry: '',
        ptime: '',
        pintroduce: '',
      }
      this.$confirm(
        '是否确认删除用户名为‘' + this.tableData[index].planName + '’的数据项',
        '提示',
        {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning',
        }
      ).then(() => {
        this.form.id = this.tableData[index].id
        this.axios.get("http://localhost:8188/NurseProject/delete/"+this.form.id, this.form)
        .then(function(response) {
                ElMessage.success({
                  message: '用户信息删除成功',
                  type: 'success',
                })
          }).catch(function(error) {
							console.log(error)
							ElMessage.success({
              message: '用户信息删除失败',
              type: 'info',
              })
					})
      }).catch(() => {
          ElMessage.success({
            message: '用户信息删除取消',
            type: 'info',
          })
				});
    },
    
    
    //条件分页查询
    findpage() {
      const state = JSON.parse(sessionStorage.getItem('state'))
      var _this = this
      this.axios({
        url: 'http://localhost:8188/NurseProject/conditionpageuser',
        method: 'post',
        data: {
          currentPage: _this.currentPage,
          pageSize: _this.pagesize,
          condition: JSON.stringify(_this.condition),
        },
      })
        .then(function (response) {
          _this.tableData = response.data.data.rows
          _this.max = response.data.data.total
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    //改变页码数
    handleCurrentChange(val) {
      this.findpage(val, this.pagesize)
    },
    //查询所有护理小组
    findallroles() {
      const state = JSON.parse(sessionStorage.getItem('state'))
      var _this = this
      this.axios({
        url: 'http://localhost:8188/NurseTeam/selectAll',
        method: 'get',
      })
        .then(function (response) {
          _this.options = response.data.data
          console.log(response)
        })
        .catch(function (error) {
          console.log(error)
        })
    },
  },
  created: function () {
    this.findpage()
    this.findallroles()
  },
}
</script>

<style>
.userlist {
  width: 100%;
  background-color: #e9eef3 !important ;
}
/* 顶部 */
.userlist .page-tag {
  height: 40px;
  padding: 0 10px;
  color: #323232;
  font-size: 18px;
  line-height: 40px;
  background-color: #e9eef3;
}
/* 内容表头 筛选框 */
.userlist-header {
  padding: 15px 15px;
  border-bottom: #e9eef3 5px solid;
  background-color: white;
}
.userlist .el-textarea .el-textarea__inner {
  resize: none;
}
/* 表体内容 */
.userlist-main {
  border-bottom: #e9eef3 5px solid;
  background-color: white;
}
.userlist th {
  background-color: #f8f8f9 !important;
  color: #515a6e !important;
}
.userlist-look th {
  color: #666666 !important;
  background-color: #e8e8e8 !important;
}
.userlist .cell {
  text-align: center;
}
/* 内容表尾 */
.userlist-footer {
  text-align: center;
}
</style>