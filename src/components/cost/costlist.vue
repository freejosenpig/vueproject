<template>
	<h1>流水界面管理</h1>
	<div>
		 <span>老人姓名:</span>
	  	 <el-input v-model="pageInfo.oldpeopleName" placeholder="老人姓名" style="width:160px;margin-bottom: 20px;"></el-input>
		 <el-button type="primary" @click="selectByname" style="width: 80px;">查询</el-button>
	</div>   
	<el-table  :data="tableData" style="width: 100%" :row-class-name="tableRowClassName">
	  <el-table-column type="index"  label="编号" align="center"></el-table-column>
	  <el-table-column  prop="jdOldpeople.oldpeopleName" label="老人姓名" align="center" ></el-table-column>
	  <el-table-column prop="listMoeny" label="消费金额" align="center" ></el-table-column>
	  <el-table-column prop="listTime" label="消费日期" align="center"> </el-table-column>
	  <el-table-column prop="listExplain" label="消费说明" align="center"> </el-table-column>
	  <el-table-column label="操作" align="center">
		  <template #default="scope">
		        <el-button size="mini" type="danger"  @click="updateListByKey(scope.row)">删除</el-button>
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
	export default{
		data(){
			return{
				pageInfo:{
					currentPage: 1,//标识当前页码
					pagesize:4,//每页多少条数据
					total:0,
					oldpeopleName:"",
				},
				tableData:[],
				prestoreForm:{
					oldpeopleName:"",oldpeopleAccount:0,prestoreMoney:"",entryfeesperson:"",oldpeopleName:""
				},
				updateform:{
					listId:""
				}
			}
		},
		methods:{
			updateListByKey(row){
				const _this=this
				this.updateform.listId=row.listId
				this.axios.put("http://localhost:8188/updateListByKey",this.updateform)
				.then(function(response){
					console.log(response)
					_this.axios.get("http://localhost:8188/selectAll",{params:_this.pageInfo})
					.then(function(response){
						console.log(response)
						_this.tableData=response.data.list
						_this.pageInfo.total = response.data.total
					}).catch(function(error){
						console.log(error)
					})
				}).catch(function(error){
					console.log(error)
				})
			},
			selectByname() {
				const _this = this
				this.axios.get("http://localhost:8188/selectBycontion", this.pageInfo)
				.then(function(response) {
					console.log(response)
					_this.tableData=response.data.list
					_this.pageInfo.total=response.data.total
				}).catch(function(error) {
					console.log(error)
				})	
			},
			handleSizeChange(pagesize) {
			    var _this=this
			    this.pageInfo.pagesize=pagesize
				var ps = qs.stringify(this.pageInfo)
				console.log(ps)
			    this.axios.get("http://localhost:8188/selectAll",{params:this.pageInfo})
			    .then(function(response){
			    	console.log("-------------------------------------------")
			    	console.log(response.data)
			    	_this.tableData=response.data.list
			    }).catch(function(error){
			    	console.log(error)
			    })
			},
			handleCurrentChange(currentPage) {
				var _this=this
				this.pageInfo.currentPage=currentPage
				var ps = qs.stringify(this.pageInfo)
				this.axios.get("http://localhost:8188/selectAll",{params:this.pageInfo})
				.then(function(response){
					console.log(response.data)
					_this.tableData=response.data.list
				}).catch(function(error){
					console.log(error)
				})
			},
			//新增预存
			// addPrestore(){
			// 	const _this=this
			// 	this.axios.post("http://localhost:8188/insertprestore",this.prestoreForm)
			// 	.then(function(response){
			// 		console.log(response)
			// 		_this.selectAllPrestore()
			// 		setTimeout(function () {
			// 		  window.location.reload()
			// 		}, 1000)
			// 	}).catch(function(error){
			// 		console.log(error)
			// 	})
			// },
			//查询所有预存信息
			// selectAllPrestore(){
			// 	const _this=this
			// 	this.axios.get("http://localhost:8188/selectPreAll")
			// 	.then(function(response){
			// 		console.log(response)
			// 		_this.tableData=response.data
			// 	}).catch(function(error){
			// 		console.log(error)
			// 	})
			// }
		},
		created() {
			const _this=this
			this.axios.get("http://localhost:8188/selectAll",{params:this.pageInfo})
			.then(function(response){
				console.log(response)
				_this.tableData=response.data.list
				_this.pageInfo.total = response.data.total
			}).catch(function(error){
				console.log(error)
			})
		}
	}
</script>

<style>
</style>
