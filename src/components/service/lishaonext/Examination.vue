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
			     prop="oldpeople_name"
			     label="老人姓名"
			     width="150">
			   </el-table-column>
			   <el-table-column
			     prop="hospital"
			     label="体检医院"
			     width="120">
			   </el-table-column>
			   <el-table-column
			     prop="etime"
			     label="体检时间"
			     width="120">
			   </el-table-column>
			   <el-table-column
			     prop="eresult"
			     label="体检结果"
			      width="120">
			         </el-table-column>
			         <el-table-column
			           prop="registrant"
			           label="登记人id"
			           width="600">
			         </el-table-column>
			         <el-table-column
			           prop="rgtime"
			           label="登记时间"
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
			              <el-button @click="updatehandleClick(scope.row)" type="text" size="small">修改</el-button>
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
				   url: "http://localhost:8188/service/ExaminationRegist",
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
			  updateUser(index, rows,row){
				  console.info(row);
				  
			  },
			  	deleteRow(index, rows,row) {
			  	  		 						        var id=row.er_Id;
			  	  		 						         this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
			  	  		 						           confirmButtonText: '确定',
			  	  		 						           cancelButtonText: '取消',
			  	  		 						           type: 'warning'
			  	  		 						         }).then(() => {
			  	  		 									 
			  	  		 									 
			  	  		 									 rows.splice(index, 1);
			  	  		 								     this.axios({
			  	  		 									 url: 'http://localhost:8188/service/ExaminationRegistdelectbyid/'+id,
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
