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
      <el-dialog title="用户修改" v-model="dialogFormVisible" width="35%" :before-close="close">
        <el-form :model="add" :rules="rules">
         <el-form-item label="员工名称：" prop="nname">
            <el-input v-model.trim="add.nname" placeholder="请填写姓名" style="width: 250px" />  
          </el-form-item>
          <el-form-item label="员工手机：" prop="nphone">
            <el-input v-model.trim="add.nphone" style="width: 250px" placeholder="请填写手机号"/>
          </el-form-item>
          <el-form-item label="员工性别：">
            <el-radio v-model="add.nsex" label="男">男</el-radio>
            <el-radio v-model="add.nsex" label="女">女</el-radio>
          </el-form-item>
		      <el-form-item label="身份证号码：" prop="ncard">
            <el-input v-model.trim="add.ncard" style="width: 250px" placeholder="请填写身份证号码" />
          </el-form-item>
          <el-form-item label="所属小组：" prop="teamId">
            <el-select v-model="add.teamId" 
            placeholder="请选择" value-key="teamId" >
             <el-option
                v-for="item in options"
                :key="item.id"
                :label="item.tname"
                :value="item.id"
              >
              </el-option>
            </el-select>
          </el-form-item>
           <el-form-item label="所属部门：" prop="ndept">
            <el-select
              v-model="add.ndept"
              placeholder="请选择">
              <el-option label="护理部" value="护理部"></el-option>
            </el-select>
            </el-form-item>
		  <el-form-item label="入职时间：">
				<input type="date" v-model="add.nworkDate" style="width: 200px;background-color: white;
					border: 1px #B3C0D1 solid;color: #8C939D;border-radius: 4px;text-align: center;" />
		  </el-form-item>
      <el-form-item label="职务：" prop="nwork">
            <el-select
              v-model="add.nwork"
              placeholder="请选择">
              <el-option label="护理" value="护理"></el-option>
            </el-select>
            </el-form-item>
          <el-form-item label="备注：">
            <el-input
              type="textarea"
              :rows="3"
              placeholder="请填写用户备注"
              v-model.trim="add.nann"
            >
            </el-input>
          </el-form-item>  
          <el-form-item label="家庭住址：" prop="naddress">
            <el-input v-model.trim="add.naddress" style="width: 250px" placeholder="请填写家庭住址" />
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
      <el-dialog title="员工新增" v-model="dialogaddVisible" width="35%">
        <el-form :model="add" :rules="rules" ref="add">
          <el-form-item label="员工名称：" prop="nname">
            <el-input v-model.trim="add.nname" placeholder="请填写姓名" style="width: 250px" />  
          </el-form-item>
          <el-form-item label="员工手机：" prop="nphone">
            <el-input v-model.trim="add.nphone" style="width: 250px" placeholder="请填写手机号"/>
          </el-form-item>
          <el-form-item label="员工性别：">
            <el-radio v-model="add.nsex" label="男">男</el-radio>
            <el-radio v-model="add.nsex" label="女">女</el-radio>
          </el-form-item>
		      <el-form-item label="身份证号码：" prop="ncard">
            <el-input v-model.trim="add.ncard" style="width: 250px" placeholder="请填写身份证号码" />
          </el-form-item>
          <el-form-item label="所属小组：" prop="teamId">
            <el-select v-model="add.teamId" 
            placeholder="请选择" value-key="teamId" >
             <el-option
                v-for="item in options"
                :key="item.id"
                :label="item.tname"
                :value="item.id"
              >
              </el-option>
            </el-select>
          </el-form-item>
           <el-form-item label="所属部门：" prop="ndept">
            <el-select
              v-model="add.ndept"
              placeholder="请选择">
              <el-option label="护理部" value="护理部"></el-option>
            </el-select>
            </el-form-item>
		  <el-form-item label="入职时间：">
				<input type="date" v-model="add.nworkDate" style="width: 200px;background-color: white;
					border: 1px #B3C0D1 solid;color: #8C939D;border-radius: 4px;text-align: center;" placeholder="请选择入职时间" />
		  </el-form-item>
      <el-form-item label="职务：" prop="nwork">
            <el-select
              v-model="add.nwork"
              placeholder="请选择">
              <el-option label="护理" value="护理"></el-option>
            </el-select>
            </el-form-item>
          <el-form-item label="备注：">
            <el-input
              type="textarea"
              :rows="3"
              placeholder="请填写用户备注"
              v-model.trim="add.nann"
            >
            </el-input>
          </el-form-item>  
          <el-form-item label="家庭住址：" prop="naddress">
            <el-input v-model.trim="add.naddress" style="width: 250px" placeholder="请填写家庭住址" />
          </el-form-item>

        </el-form>
        <template #footer>
          <span class="dialog-footer">
            <el-button @click="dialogaddVisible = false">取 消</el-button>
            <el-button type="primary" @click="addnewok('add')">确 定</el-button>
          </span>
        </template>
      </el-dialog>
  <!-- 查看 -->
   <el-dialog title="查看员工" v-model="dialogFormVisible1" width="35%" :before-close="close">
        <el-form :model="form" :rules="rules">
         <el-form-item label="员工名称：" prop="nname">
            <el-input v-model.trim="add.nname" placeholder="请填写姓名" style="width: 250px" disabled="true"/>  
          </el-form-item>
          <el-form-item label="员工手机：" prop="nphone">
            <el-input v-model.trim="add.nphone" style="width: 250px" placeholder="请填写手机号" disabled="true"/>
          </el-form-item>
          <el-form-item label="员工性别：" >
            <el-radio v-model="add.nsex" label="男" disabled="true">男</el-radio>
            <el-radio v-model="add.nsex" label="女" disabled="true">女</el-radio>
          </el-form-item>
		      <el-form-item label="身份证号码：" prop="ncard">
            <el-input v-model.trim="add.ncard" style="width: 250px" placeholder="请填写身份证号码" disabled="true" />
          </el-form-item>
          <el-form-item label="所属小组：" prop="teamId">
            <el-select v-model="add.teamId" 
            placeholder="请选择" value-key="teamId" disabled="true" >
             <el-option
                v-for="item in options"
                :key="item.id"
                :label="item.tname"
                :value="item.id"
              >
              </el-option>
            </el-select>
          </el-form-item>
           <el-form-item label="所属部门：" prop="ndept" disabled="true">
            <el-select
              v-model="add.ndept"
              placeholder="请选择" disabled="true">
              <el-option label="护理部" value="护理部"></el-option>
            </el-select>
            </el-form-item>
		  <el-form-item label="入职时间：" >
				<input type="date" v-model="add.nworkDate" style="width: 200px;background-color: white;
					border: 1px #B3C0D1 solid;color: #8C939D;border-radius: 4px;text-align: center;"   disabled="true" />
		  </el-form-item>
      <el-form-item label="职务：" prop="nwork" >
            <el-select
              v-model="add.nwork"
              placeholder="请选择" disabled="true">
              <el-option label="护理" value="护理"></el-option>
            </el-select>
            </el-form-item>
          <el-form-item label="备注：">
            <el-input
              type="textarea"
              :rows="3"
              placeholder="请填写用户备注"
              v-model.trim="add.nann"
              disabled="true"
            >
            </el-input>
          </el-form-item>  
          <el-form-item label="家庭住址：" prop="naddress">
            <el-input v-model.trim="add.naddress" style="width: 250px" placeholder="请填写家庭住址" disabled="true" />
          </el-form-item>

        </el-form>
        <template #footer>
          <span class="dialog-footer">
            <el-button @click="dialogFormVisible1 = false">取 消</el-button>
          </span>
        </template>
      </el-dialog> 

      <div style="padding: 10px 25px">
        <el-button
          icon="el-icon-plus"
          type="primary"
          size="large"
          @click="addnew()"
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
            员工性别：
		  <el-select v-model="vagueorderid2" placeholder="请选择" clearable  style="higt:100px" >
				<el-option label="男" value="男"></el-option>
				<el-option label="女" value="女"></el-option> 
            </el-select>
          <el-button icon="el-icon-search" size="large" @click="join()" 
            >查询</el-button
          >
        </div>
      </div>
      <el-table :data="tableData" style="width: 100%; margin-bottom: 20px">
        <el-table-column prop="id" label="员工编号" />
        <el-table-column prop="nname" label="员工名称" />
        <el-table-column prop="nsex" label="员工性别" />
        <el-table-column prop="nphone" label="员工手机号" />
        <el-table-column prop="nworkDate" label="入职时间"/>
        <el-table-column prop="teamId" label="所属小组ID" width="200px" />
        <el-table-column prop="nwork" label="职务" width="200px" />
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
            <!-- click 方法未修改！！！！！！ -->
            <el-button
              type="text"
              class="el-icon-info"
              size="samll"
             @click="selectshow(scope.row)"
              >详情</el-button
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
        nname: '',
        nphone: '',
		    nage:'',
        nsex: '',
		    nnation:'',
	  	  ncard:'',
        naddress:'',
        nephone:'',
        nwork:'',
        ntype:'',
        ndept:'',
        nimg:'',
        nworkDate: '',
        createpeople: '',
		    createtime:'',
        nann: '',
        teamId: '',
      },
      add: {
        nname: '',
        nphone: '',
        nsex: '男',
        nworkDate : '',
        ncard:'',
        nann: '',
        naddress:'',
		    createPeople: '',
        teamId: '',
      },
      //新增form表单规则
      rules: {
        nname: [
          { required: true, message: '请输入用户名称', trigger: 'blur' },
          {
            min: 2,
            max: 10,
            message: '长度在 2 到 10 个字符',
            trigger: 'blur',
          },
        ],
        nphone: [
          { required: true, message: '请输入手机号', trigger: 'blur' },
          {
            pattern: /^1[34578]\d{9}$/, //正则校验不用字符串
            message: '请填写正确的手机号码',
            trigger: 'blur',
          },
        ],
        ncard:[
          {required: true, message: '请输入身份证号', trigger: 'blur'},
           {
            pattern: /^[1-9]\d{5}(18|19|([23]\d))\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$/, //正则校验不用字符串
            message: '请填写正确的身份证号码',
            trigger: 'blur',
          },
        ],

      },
      //修改form表单规则
      rules1: {
        nname: [
          { required: true, message: '请输入用户名称', trigger: 'blur' },
          {
            min: 2,
            max: 10,
            message: '长度在 2 到 10 个字符',
            trigger: 'blur',
          },
        ],
        nphone: [
          { required: true, message: '请输入手机号', trigger: 'blur' },
          {
            pattern: /^1[34578]\d{9}$/, //正则校验不用字符串
            message: '请填写正确的手机号码',
            trigger: 'blur',
          },
        ],
        ncard:[
          {required: true, message: '请输入身份证号', trigger: 'blur'},
           {
            pattern: /^[1-9]\d{5}(18|19|([23]\d))\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$/, //正则校验不用字符串
            message: '请填写正确的身份证号码',
            trigger: 'blur',
          },
        ],

      },
      options: [],
      condition: { nname: '' , nsex:''}, //查询条件
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
      this.condition.nname = this.vagueorderid,
	  this.condition.nsex = this.vagueorderid2
      this.findpage()
    },
    //新增
    addnew() {
      this.add = {
        nname: '',
        nphone: '',
        nsex: '男',
        nworkDate: '',
        anno:'',
        teamId:'',
      }
      this.dialogaddVisible = true
    },
    //确认添加
    addnewok(formName) {
	  var _this = this
		this.add.createPeople = this.$store.state.userInfo.userName
		this.axios.post("http://localhost:8188/serviceNurse/addNurse",this.add)      
        .then(function(response) {
						console.log(response)
							ElMessage.success({
                    		message: '用户信息添加成功',
                    		type: 'success',
                  			})
            _this.dialogaddVisible = false;
						_this.axios.get("http://localhost:8188/serviceNurse/selectAll")
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
    //查看显示
    selectshow(row) {
				this.add.id = row.id
        this.add.nname = row.nname
       	this.add.nphone = row.nphone
       	this.add.nsex = row.nsex
       	this.add.teamId = row.teamId
       	this.add.nnation = row.nnation
       	this.add.ncard = row.ncard
        this.add.naddress = row.naddress
        this.add.nephone = row.nephone
        this.add.nwork = row.nwork
        this.add.ntype = row.ntype
       	this.add.ndept = row.ndept
        this.add.nimg = row.nimg
        this.add.createpeople = row.createpeople
        this.add.createtime = row.createtime
        this.add.nworkDate = row.nworkDate	
        this.add.nann= row.nann
				this.dialogFormVisible1 = true
			},
    //查询

    //修改显示
    updateconshow(row) {
				this.add.id = row.id
        this.add.nname = row.nname
       	this.add.nphone = row.nphone
       	this.add.nsex = row.nsex
       	this.add.teamId = row.teamId
       	this.add.nnation = row.nnation
       	this.add.ncard = row.ncard
        this.add.naddress = row.naddress
        this.add.nephone = row.nephone
        this.add.nwork = row.nwork
        this.add.ntype = row.ntype
       	this.add.ndept = row.ndept
        this.add.nimg = row.nimg
        this.add.createpeople = row.createpeople
        this.add.createtime = row.createtime
        this.add.nworkDate = row.nworkDate	
        this.add.nann= row.nann
				this.dialogFormVisible = true
			},
    //修改
    update() {
      var _this = this
      console.log(_this.form)
      this.axios.put("http://localhost:8188/serviceNurse/updateNurse", this.add)
        .then(function (response) {
         console.log(response)
            	ElMessage.success({
                    		message: '用户信息添加成功',
                    		type: 'success',
                  			})
						_this.dialogFormVisible = false
						_this.axios.get("http://localhost:8188/serviceNurse/selectAll", {
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
        nname: '',
        nphone: '',
		    nage:'',
        nsex: '',
		    nnation:'',
		    n_card:'',
		    naddress:'',
		    nephone:'',
		    nwork:'',
		    ntype:'',
		    ndept:'',
		    n_img:'',
        nworkDate: '',
        createpeople: '',
		    createtime:'',
        nann: '',
        teamId: '',
      }
      this.$confirm(
        '是否确认删除用户名为‘' + this.tableData[index].nname + '’的数据项',
        '提示',
        {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning',
        }
      ).then(() => {
        this.form.id = this.tableData[index].id
        this.axios.get("http://localhost:8188/serviceNurse/delete/"+this.form.id, this.form)
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
        url: 'http://localhost:8188/serviceNurse/conditionpageuser',
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