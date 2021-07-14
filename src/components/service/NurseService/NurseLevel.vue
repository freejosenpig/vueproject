<template>
  <!-- 主内容 -->
  <div class="userlist">
    <!-- 标题 -->
    <div class="page-tag">
      <span style="float: left">护理级别管理</span>
      <!-- 标签页 -->
    </div>
    <!-- 表体内容 -->
    <div class="userlist-mian">
       <!-- 修改 -->
      <el-dialog title="级别修改" v-model="dialogFormVisible" width="35%" :before-close="close">
        <el-form :model="add" >
            <el-form-item label="级别名称：" prop="lname">
            <el-input v-model.trim="add.lname" placeholder="请填写名称" style="width: 250px" />  
            </el-form-item>
           <el-form-item label="护理类型：">
            <el-radio v-model="add.ltype" label="自理">自理</el-radio>
            <el-radio v-model="add.ltype" label="半自理">半自理</el-radio>
            <el-radio v-model="add.ltype" label="不能自理">不能自理</el-radio>
            </el-form-item>
            <el-form-item label="护理费用：" prop="lmoney">
            <el-input v-model.trim="add.lmoney" style="width: 250px" placeholder="请填写费用"/>
            </el-form-item>
		    <el-form-item label="生效时间：">
				<input type="date" v-model="add.ldate" style="width: 200px;background-color: white;
					border: 1px #B3C0D1 solid;color: #8C939D;border-radius: 4px;text-align: center;" placeholder="请选择时间" />
		     </el-form-item>
            <el-form-item label="级别说明：">
            <el-input
              type="textarea"
              :rows="3"
              placeholder="请填写说明"
              v-model.trim="add.lexplain"
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
      <el-dialog title="新增级别" v-model="dialogaddVisible" width="35%">
        <el-form :model="add" ref="add">
          <el-form-item label="级别名称：" prop="lname">
            <el-input v-model.trim="add.lname" placeholder="请填写名称" style="width: 250px" />  
          </el-form-item>
           <el-form-item label="护理类型：">
            <el-radio v-model="add.ltype" label="自理">自理</el-radio>
            <el-radio v-model="add.ltype" label="介助">介助</el-radio>
            <el-radio v-model="add.ltype" label="不能自理">介护</el-radio>
          </el-form-item>
          <el-form-item label="护理费用：" prop="lmoney">
            <el-input v-model.trim="add.lmoney" style="width: 250px" placeholder="请填写费用"/>
          </el-form-item>
		  <el-form-item label="生效时间：">
				<input type="date" v-model="add.ldate" style="width: 200px;background-color: white;
					border: 1px #B3C0D1 solid;color: #8C939D;border-radius: 4px;text-align: center;" placeholder="请选择时间" />
		  </el-form-item>
          <el-form-item label="级别说明：">
            <el-input
              type="textarea"
              :rows="3"
              placeholder="请填写说明"
              v-model.trim="add.lexplain"
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
          @click="addnew"
          >新增用户</el-button
        >
        <!-- 模糊查询 -->
        <div style="float: right">
            员工名称：
          <el-input
            clearable
            v-model="vagueorderid"
            placeholder=""
            style="width: 200px"
            size="large"
          />
          <el-button icon="el-icon-search" size="large" @click="join()" 
            >查询</el-button
          >
        </div>
      </div>
      <el-table :data="tableData" style="width: 100%; margin-bottom: 20px">
        <el-table-column prop="id" label="级别编号" />
        <el-table-column prop="lname" label="级别名称" />
        <el-table-column prop="ltype" label="护理类型" />
        <el-table-column prop="lmoney" label="护理费用" />
        <el-table-column prop="ldate" label="生效日期"/>
        <el-table-column prop="lexplain" label="级别说明" width="200px" />
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
      dialogaddVisible:false,
      form: {
        id: '',
        lname: '',
        ltype: '',
        lmoney: '',
        ldate: '',
        lexplain: '',

      },
      add: {
        id: '',
        lname: '',
        ltype: '',
        lmoney: '',
        ldate: '',
        lexplain: '',
      },
      options: [],
      condition: { lname: '' ,}, //查询条件
      thisusername: '',
      thisuserphone: '',
      vagueorderid: '',
      //分页
      pagesize: 10,
      max: 0,
      currentPage: 1,
    }
  },
  methods: {
    //模糊查询
    join() {
      this.condition.lname = this.vagueorderid
      this.findpage()
    },
    //新增
    addnew() {
      this.add = {
        id: '',
        lname: '',
        ltype: '',
        lmoney: '',
        ldate: '',
        lexplain: '',
      }
      this.dialogaddVisible = true
    },
    //确认添加
    addnewok(formName) {
	  var _this = this
		this.add.createPeople = this.$store.state.userInfo.userName
		this.axios.post("http://localhost:8188/NurseLevel/addNurse",this.add)      
        .then(function(response) {
						console.log(response)
							ElMessage.success({
                    		message: '用户信息添加成功',
                    		type: 'success',
                  			})
            _this.dialogaddVisible = false;
						_this.axios.get("http://localhost:8188/NurseLevel/selectAll")
            .then(function(response) {
							console.log(response)
							_this.tableData = response.data.list			
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
        this.add.lname = row.lname
       	this.add.ltype = row.ltype
       	this.add.lmoney = row.lmoney
       	this.add.ldate = row.ldate
       	this.add.lexplain = row.lexplain
      
		this.dialogFormVisible = true
			},
    //修改
    update() {
      var _this = this
      console.log(_this.form)
      this.axios.post("http://localhost:8188/NurseLevel/updateNurse", this.add)
        .then(function (response) {
         console.log(response)
            	ElMessage.success({
                    		message: '用户信息添加成功',
                    		type: 'success',
                  			})
						_this.dialogFormVisible = false
						_this.axios.get("http://localhost:8188/NurseLevel/selectAll", {
						}).then(function(response) {
							console.log(response)
							_this.tableData = response.data.list
							//_this.pageInfo1.total = response.data.total
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
        lname: '',
        ltype: '',
        lmoney: '',
        ldate: '',
        lexplain: '',
      }
      this.$confirm(
        '是否确认删除用户名为‘' + this.tableData[index].lname + '’的数据项',
        '提示',
        {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning',
        }
      ).then(() => {
        this.form.id = this.tableData[index].id
        this.axios.get("http://localhost:8188/NurseLevel/delete/"+this.form.id, this.form)
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
        //this.form.delFlag = -1
        //this.update(-1)
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
        url: 'http://localhost:8188/NurseLevel/conditionpageuser',
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