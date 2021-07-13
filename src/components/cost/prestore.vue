<template>
	<h1>预存界面管理</h1>
	    <div>
		  <el-form :inline="true" :model="formInline" class="demo-form-inline">
		  	  <el-form-item label="老人姓名">
		  		<el-input v-model="formInline.user" placeholder="老人姓名"></el-input>
		  	  </el-form-item>
		  	  <el-form-item label="预存金额">
		  		<el-input v-model="formInline.user" style="width: 100px;"/>-<el-input v-model="formInline.user"  style="width: 100px;"/>
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
				<el-select v-model="oldpeopleName" style="width: 130px;">
					<el-option v-for="old in olddata" :value="old.oldpeopleId" :label="old.oldpeopleName"></el-option>
				 </el-select>
				 预存金额:
				 <el-input v-model="prestoreMoney" style="width: 130px;"></el-input>
				 <el-button @click="addPrestore" style="width: 130px;margin-left: 20px;" type="primary">确定预存</el-button>
				 <el-button @click="addcancel" style="width: 130px;margin-left: 20px;" type="danger">取消预存</el-button>
			</el-col>
		</div>
	    <el-table  :data="tableData" style="width: 100%" :row-class-name="tableRowClassName">
		  <el-table-column type="index"  label="编号" align="center">  </el-table-column>
	      <el-table-column  prop="jdOldpeople.oldpeopleName" label="老人姓名" align="center" >  </el-table-column>
	      <el-table-column prop="jdOldpeople.oldpeopleAccount" label="账户金额" align="center" ></el-table-column>
	      <el-table-column prop="prestoreMoney" label="预存金额" align="center"> </el-table-column>
		  <el-table-column label="操作" align="center">
			  <template #default="scope">
			        <el-button size="mini" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
			        <el-button   size="mini" type="danger"  @click="handleDelete(scope.$index, scope.row)">删除</el-button>
			  </template>
		  </el-table-column>
	    </el-table>
</template>

<script>
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
					oldpeopleName:"",oldpeopleAccount:0,prestoreMoney:"",entryfeesperson:"",oldpeopleName:""
				}
			}
		},
		methods:{
			addcancel(){
				this.styleObject.display='none'
			},
			showaddiv(){
				this.styleObject.display='block'
			},
			//新增预存
			addPrestore(){
				const _this=this
				this.axios.post("http://localhost:8188/insertprestore",this.prestoreForm)
				.then(function(response){
					console.log(response)
					_this.selectAllPrestore()
					setTimeout(function () {
					  window.location.reload()
					}, 1000)
				}).catch(function(error){
					console.log(error)
				})
			},
			//查询所有预存信息
			selectAllPrestore(){
				const _this=this
				this.axios.get("http://localhost:8188/selectPreAll")
				.then(function(response){
					console.log(response)
					_this.tableData=response.data
				}).catch(function(error){
					console.log(error)
				})
			}
		},
		created() {
			const _this=this
			this.axios.get("http://localhost:8188/selectPreAll")
			.then(function(response){
				console.log(response)
				_this.tableData=response.data
			}).catch(function(error){
				console.log(error)
			})
			this.axios.get("http://localhost:8188/selectAllOldpeople")
			.then(function(response){
				console.log(response)
				_this.olddata=response.data
			}).catch(function(error){
				console.log(error)
			})
		}
	}
</script>

<style>
</style>
