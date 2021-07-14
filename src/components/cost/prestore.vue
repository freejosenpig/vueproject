<template>
	<h1>预存界面管理</h1>
	    <div>
		  <el-form :inline="true" :model="formInline" class="demo-form-inline">
		  	  <el-form-item label="老人姓名">
		  		<el-input v-model="formInline.user" placeholder="老人姓名"></el-input>
		  	  </el-form-item>
		  	  <el-form-item>
		  		<el-button type="primary" @click="onSubmit">查询</el-button>
		  	  </el-form-item>
			  <el-button @click="showaddiv" type="primary">新增预存</el-button>
		  </el-form>
	    </div>
		<!-- 新增预存表单 -->
		<div style="width:100%;height: 50px;" class="addiv"  v-bind:style="styleObject">
			<el-col>
				预存老人:
				<el-select v-model="prestoreForm.oldpeopleId" style="width: 130px;">
					<el-option v-for="old in olddata" :value="old.oldpeopleId" :label="old.oldpeopleName"></el-option>
				 </el-select>
				 预存金额:
				 <el-input v-model="prestoreForm.prestoreMoney" style="width: 130px;"></el-input>
				 <el-button @click="addPrestore" style="width: 130px;margin-left: 20px;" type="primary">确定预存</el-button>
				 <el-button @click="addcancel" style="width: 130px;margin-left: 20px;" type="danger">取消预存</el-button>
			</el-col>
		</div>
	    <el-table  :data="tableData" style="width: 100%" :row-class-name="tableRowClassName">
		  <el-table-column type="index"  label="编号" align="center">  </el-table-column>
	      <el-table-column  prop="jdOldpeople.oldpeopleName" label="老人姓名" align="center" ></el-table-column>
	      <el-table-column prop="jdOldpeople.oldpeopleAccount" label="账户金额" align="center" ></el-table-column>
	      <el-table-column prop="prestoreMoney" label="预存金额" align="center"> </el-table-column>
		  <el-table-column label="操作" align="center">
			  <template #default="scope">
						<el-button size="mini" type="danger" @click="updatePreByKey(scope.row)">删除</el-button>
					<!-- </p> -->
			  </template>
		  </el-table-column>
	    </el-table>
		<!-- 分页 -->
		<div class="block" style="margin-left: -12px; margin-top: 10px;">
		    <el-pagination @size-change="handleSizeChange" background @current-change="handleCurrentChange"
		      v-model:currentPage="currentPage" :page-sizes="[2, 4, 6, 8]" :page-size="pageInfo.pagesize"
		      layout="sizes, prev, pager, next" :total="pageInfo.total">
		    </el-pagination>
		  </div>
</template>

<script>
	import qs from 'qs'
	import { ElMessage } from 'element-plus'
	export default{
		data(){
			return{
				formInline:{
					oldpeopleName:"",
					value1:"",value2:""
				},
				styleObject: {
				    display: 'none'
				},
				//老人信息
				olddata:[],
				tableData:[],
				prestoreForm:{
					oldpeopleName:"",oldpeopleAccount:0,prestoreMoney:0,entryfeesperson:"",oldpeopleId:""
				},
				updateAccount1:{
					oldpeopleId:"",oldpeopleAccount:""
				},
				totalAccount:0,
				moneys:0,
				pageInfo:{
					currentPage: 1,//标识当前页码
					pagesize:4,//每页多少条数据
					total:0,
					oldpeopleName:"",
				},
				updateform:{
					prestoreId:""
				}
			}
		},
		methods:{
			//修改预存管理
			updatePreByKey(row){
				const _this=this
				this.updateform.prestoreId=row.prestoreId
				this.$confirm('此操作将删除该数据, 是否继续?', '提示', {
						confirmButtonText: '确定',
						cancelButtonText: '取消',
						type: 'warning'
					}).then(()=>{
						_this.axios.put("http://localhost:8188/updatePreByKey",this.updateform)
				.then(function(response){
					_this.axios.get("http://localhost:8188/selectPreAll",{params:_this.pageInfo})
							.then(function(response) {
								console.log(response)
								_this.tableData=response.data.list
								_this.pageInfo.total = response.data.total
							}).catch(function(error) {
								console.log(error)
							})
							ElMessage.success({
							   message: '删除成功',
							   type: 'success'
							})
						}).catch(function(error){
							console.log(error)
				      })
				}).catch(function(){
					this.$message({
						type: 'error',
						message: '取消删除!'
					});
				})
				
				
				// const _this=this
				// this.updateform.prestoreId=row.prestoreId
				// this.axios.put("http://localhost:8188/updatePreByKey",this.updateform)
				// .then(function(response){
				// 	console.log(response)
				// 	_this.axios.get("http://localhost:8188/selectPreAll",{params:_this.pageInfo})
				// 	.then(function(response){
				// 		console.log(response)
				// 		_this.tableData=response.data.list
				// 		_this.pageInfo.total = response.data.total
				// 	}).catch(function(error){
				// 		console.log(error)
				// 	})
				// 	ElMessage.success({
				// 	   message: '删除成功',
				// 	   type: 'success'
				// 	})
				// }).catch(function(error){
				// 	console.log(error)
				// })
			},
			handleSizeChange(pagesize) {
			    var _this=this
			    this.pageInfo.pagesize=pagesize
				var ps = qs.stringify(this.pageInfo)
				console.log(ps)
			    this.axios.get("http://localhost:8188/selectPreAll",{params:this.pageInfo})
			    .then(function(response){
			    	console.log("-------------------------------------------")
			    	console.log(response.data)
			    	_this.tableData=response.data.data.list
			    }).catch(function(error){
			    	console.log(error)
			    })
			},
			handleCurrentChange(currentPage) {
				var _this=this
				this.pageInfo.currentPage=currentPage
				var ps = qs.stringify(this.pageInfo)
				this.axios.get("http://localhost:8188/selectPreAll",{params:this.pageInfo})
				.then(function(response){
					console.log(response.data)
					_this.tableData=response.data.data.list
				}).catch(function(error){
					console.log(error)
				})
			},
			addcancel(){
				this.styleObject.display='none'
			},
			showaddiv(){
				this.styleObject.display='block'
			},
			//根据老人编号修改账户余额
			updateAccount(OldId,account){
				const _this=this
				this.updateAccount1.oldpeopleId=OldId
				console.log(this.updateAccount1.oldpeopleId+"bbbb")
				console.log(account+"ccc")
				this.moneys=account
				//根据老人编号查询所有老人信息
				this.axios.get("http://localhost:8188/selectByPrimaryKey/"+this.updateAccount1.oldpeopleId)
				.then(function(response){
					console.log(response)
					_this.totalAccount=response.data.data.oldpeopleAccount
					console.log(_this.totalAccount+"总账户")
					console.log(response.data.data.oldpeopleAccount+"总账户")
					_this.updateAccount1.oldpeopleAccount=parseFloat(_this.totalAccount)+parseFloat(_this.moneys)
					console.log(_this.updateAccount1.oldpeopleAccount+"剩余金额")
					_this.axios.put("http://localhost:8188/updateAccount",_this.updateAccount1)
					.then(function(response){
						console.log(response)
					}).catch(function(error){
						console.log(error)
					})
				}).catch(function(error){
					console.log(error)
				})
				
			},
			//新增预存
			addPrestore(){
				const _this=this
				this.axios.post("http://localhost:8188/insertprestore",this.prestoreForm)
				.then(function(response){
					console.log(response)
					_this.selectAllPrestore()
					_this.updateAccount(_this.prestoreForm.oldpeopleId,_this.prestoreForm.prestoreMoney)
					setTimeout(function () {
					  window.location.reload()
					}, 1000)
					_this.addcancel()
				}).catch(function(error){
					console.log(error)
				})
			},
			//查询所有预存信息
			selectAllPrestore(){
				const _this=this
				this.axios.get("http://localhost:8188/selectPreAll",{params:this.pageInfo})
				.then(function(response){
					console.log(response)
					_this.tableData=response.data.data.list
					_this.pageInfo.total = response.data.data.total
				}).catch(function(error){
					console.log(error)
				})
			}
		},
		created() {
			const _this=this
			this.axios.get("http://localhost:8188/selectPreAll",{params:this.pageInfo})
			.then(function(response){
				console.log(response)
				_this.tableData=response.data.data.list
				_this.pageInfo.total = response.data.data.total
			}).catch(function(error){
				console.log(error)
			})
			this.axios.get("http://localhost:8188/selectAllOldpeople")
			.then(function(response){
				console.log(response)
				_this.olddata=response.data.data
			}).catch(function(error){
				console.log(error)
			})
		}
	}
</script>

<style>
</style>
