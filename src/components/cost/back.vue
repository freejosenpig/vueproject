<template>
	<h1>退住界面管理</h1>
	<div>
	  <el-form :inline="true" :model="formInline" class="demo-form-inline">
	  	  <el-form-item label="老人姓名">
	  		<el-input v-model="formInline.user" placeholder="老人姓名"></el-input>
	  	  </el-form-item>
	  	  <el-form-item label="退住金额">
	  		<el-input v-model="formInline.user" style="width: 100px;"/>-<el-input v-model="formInline.user"  style="width: 100px;"/>
	  	  </el-form-item>
	  	  <el-form-item>
	  		<el-button type="primary" @click="onSubmit">查询</el-button>
	  	  </el-form-item>
	  </el-form>
	</div>
	
	<el-table  :data="tableData" style="width: 100%" :row-class-name="tableRowClassName">
	  <el-table-column type="index"  label="编号" align="center">  </el-table-column>
	  <el-table-column  prop="jdOldpeople.oldpeopleName" label="老人姓名" align="center" >  </el-table-column>
	  <el-table-column prop="jdOldpeople.oldpeopleAccount" label="入院时间" align="center" ></el-table-column>
	  <el-table-column prop="prestoreMoney" label="结算时间" align="center"> </el-table-column>
	  <el-table-column prop="prestoreMoney" label="结算状态" align="center"> </el-table-column>
	  <el-table-column label="操作" align="center">
		  <template #default="scope">
		        <el-button size="mini" @click="handleEdit(scope.$index,scope.row)">编辑</el-button>
		        <el-button size="mini" type="danger"  @click="handleDelete(scope.$index, scope.row)">删除</el-button>
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
				tableData:[]
			}
		},
		methods:{
			
		},
		created() {
			const _this=this
			this.axios.get("http://localhost:8188/selectBackAll")
			.then(function(response){
				console.log(response)
				_this.tableData=response.data
			}).catch(function(error){
				console.log(error)
			})
		}
	}
</script>

<style>
</style>
