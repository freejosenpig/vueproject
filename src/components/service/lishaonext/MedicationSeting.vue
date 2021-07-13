<template>
	<div >
			<el-row>
				 <el-col :span="6"> <el-input
				     size="medium"
				     placeholder="查询姓名"
				     v-model="input4" >
				     <template #prefix>
				       <i class="el-input__icon el-icon-search"></i>
				     </template>
				   </el-input> 
				   </el-col>
				  <el-col :span="18"><el-button icon="el-icon-search" circle></el-button></el-col>
			</el-row>
	        <el-table
	           :data="tableData"
	           border
	           style="width: 100%">
			   <el-table-column
			     prop="oldpeople_Name"
			     label="老人姓名"
			     width="150">
			   </el-table-column>
			   <el-table-column
			     prop="disease"
			     label="疾病"
			     width="120">
			   </el-table-column>
			   <el-table-column
			     prop="diseasetype"
			     label="疾病类型"
			     width="120">
			   </el-table-column>
			   	  <el-table-column
			   	    prop="create_time"
			   	    label="创建时间"
			   	    width="120">
			   	  </el-table-column>
			         <el-table-column
			           fixed="right"
			           label="操作"
			           width="100">
			           <template #default="scope">
			             <el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
			              <el-button type="text" size="small" @click="deleteRow(scope.$index, tableData,scope.row)" >销户</el-button>
			           </template>
			         </el-table-column>
			       </el-table>
	 <div class="block">
	    <span class="demonstration">直接前往</span>
	    <el-pagination
	       @size-change="handleSizeChange"
	       @current-change="handleCurrentChange"
	       v-model:currentPage="requestParameters.page"
	       :page-size="requestParameters.size"
	       layout="prev, pager, next, jumper"
	       :total="counts">
	    </el-pagination>
	  </div>
	</div>
</template>

<script>
	export default {
	    data() {
	      return {
			 currentPage3:1,
	        tableData:[],
			counts: '',
			requestParameters:{
			  page: 1,
			  size: 10,
			}
	      }
	    },
		created:function(){
		   this.doQuery();
								
		},
		 methods: {
			 
			 // 业务方法
			 doQuery(params){
				 var that=this;
				 this.axios({
				   url: "http://localhost:8188/service/medicaSetingImp",
				   method: "get",
				   data:that.requestParameters
				 }).then(function(res){
								console.log(res.data.data);
				 		        that.tableData= res.data.data;
				 		        that.counts = res.data.data.length;
				 		      }).catch(function(err) {
				 			 			     console.log("上传失败");
				 			 			     console.log(err);
				 			  })
				 
			    
			 },
			 // 每页显示信息条数
			 handleSizeChange(size) {
			   this.requestParameters.size = size
			   if (this.requestParameters.page === 1) {
			     this.doQuery(this.requestParameters)
			   }
			 },
			 // 进入某一页
			 handleCurrentChange(val) {
			   this.requestParameters.page = val
			   this.doQuery()
			 },
			   handleClick(row) {
			         console.log(row);
			    },
		      handleClick(row) {
		        console.log(row);
		      },
			  		 deleteRow(index, rows,row) {
			  		   		 						        var id=row.ms_Id;
			  		   		 						         this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
			  		   		 						           confirmButtonText: '确定',
			  		   		 						           cancelButtonText: '取消',
			  		   		 						           type: 'warning'
			  		   		 						         }).then(() => {
			  		   		 									 
			  		   		 									 
			  		   		 									 rows.splice(index, 1);
			  		   		 								     this.axios({
			  		   		 									 url: 'http://localhost:8188/service/medicalRegisterdelectbyid/'+id,
			  		   		 									 method: 'DELETE'
			  		   		 				  						 			
			  		   		 									 }).then(function(response) {
			  		   		 									 			  			 				
			  		   		 									 console.log(id)
			  		   		 									 console.log(typeof id)
			  		   		 									 			  			 													  
			  		   		 									console.log("提交")
			  		   		 									 console.log(response)
			  		   		 									 	  								 		
			  		   		 									 }).catch(function(err) {
			  		   		 								      console.log("上传失败")
			  		   		 									 console.log(id)	
			  		   		 									  console.log(err)
			  		   		 									 	})
			  		   		 									 
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
			  		   		 			 			 
			  		   		 			 											 	   	
			  		   		 			 											 
			  		   		 			 			
			  		   		 			       }
			  		   		 							   
			  		   		 						  
			  		   }
			  		 }
	</script>
	


<style>
</style>
