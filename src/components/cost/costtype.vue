<template>
	<h1>费用类别设置</h1>
	<el-table :data="tabledata" style="width: 100%">
		<!-- <el-table-column type="expand">
			<template #default="props"> -->
				<!-- <el-form label-position="left" inline class="demo-table-expand">
					<el-form-item label="类型详情">
					    <span>{{ props.row.typeName }}</span>
					</el-form-item>
					<el-form-item label="价格">
					    <span>{{ props.row.typeOpid }}</span>
					</el-form-item>
					<el-form-item label="描述">
					    <span>{{ props.row.typeTime }}</span>
					</el-form-item>
				</el-form> -->
				<!-- <el-table :data=""></el-table> -->
		<!-- 	</template>
		</el-table-column> -->
	    <el-table-column label="类别名称" prop="typeName" align="center" ></el-table-column>
	    <el-table-column  label="新增时间" prop="typeTime"  align="center"> </el-table-column>
		<el-table-column  label="新增人" prop="typeOpid"  align="center"> </el-table-column>
	    <el-table-column align="right">
	      <template #header>
	        <el-input v-model="search" size="mini" placeholder="输入关键字搜索"/>
	      </template>
	      <template #default="scope">
	        <el-button size="mini" @click="handleEdit(scope.$index, scope.row)">Edit</el-button>
			<el-popconfirm  title="这是一段内容确定删除吗？" >
			<template #reference>
			  <el-button>删除</el-button>
			  </template>
			</el-popconfirm>
	        <!-- <el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">Delete</el-button> -->
	      </template>
	    </el-table-column>
	  </el-table>
</template>

<script>
	export default{
		name:"costtype",
		data(){
			return{
				search: '',
				tabledata:[
					// details:[]
				]
			}
		},
		methods: {
		    handleEdit(index, row) {
		       console.log(index, row);
		    },
		    handleDelete(index, row) {
		        console.log(index, row);
		    }
		},
		created() {
			const _this=this
			this.axios.get("http://localhost:8188/findAlldetails")
			.then(function(response){
				console.log(response)
				// _this.tabledata.details=response.co
				_this.tabledata=response.data
			}).catch(function(error){
				console.log(error)
			})
		}
	}
</script>

<style>
</style>
