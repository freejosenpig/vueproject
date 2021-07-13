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
			    fixed
	             prop="oldpeople_Name"
	             label="老人姓名"
	             width="150">
	           </el-table-column>
	           <el-table-column
	             prop="paymentmethod"
	             label="缴药方式"
	             width="120">
	           </el-table-column>
	           <el-table-column
	             prop="dgname"
	             label="药名"
	             width="120">
	           </el-table-column>
	           <el-table-column
	             prop="number"
	             label="数量"
	              width="120">
	                 </el-table-column>
	                 <el-table-column
	                   prop="munit"
	                   label="用药单位"
	                   width="100">
	                 </el-table-column>
	                 <el-table-column
	                   prop="status"
	                   label="状态"
	                   width="120">
	                 </el-table-column>
	           	  <el-table-column
	           	    prop="rname"
	           	    label="登记人"
	           	    width="120">
	           	  </el-table-column>
	           	  <el-table-column
	           	    prop="creat_Time"
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
				     <div v-show="visible" class="visible">
					 <el-form ref="form" :model="form" label-width="80px">
					   <el-form-item label="缴药方式">
					     <el-input v-model="form.paymentmethod"></el-input>
					   </el-form-item>
					  
					    <el-form-item label="药名">
					      <el-input v-model="form.dgname"></el-input>
					    </el-form-item>
						
						  <el-form-item label="数量">
						    <el-input v-model="form.number"></el-input>
						  </el-form-item>
						  <el-form-item label="用药单位">
						    <el-input v-model="form.munit"></el-input>
						  </el-form-item>
						  <el-form-item label="用药状态">
						    <el-input v-model="form.status"></el-input>
						  </el-form-item>
						  <el-form-item label="登记人">
						    <el-input v-model="form.rname"></el-input>
						  </el-form-item>
					   <el-form-item>
					       <el-button type="primary" @click="onSubmit">立即修改</el-button>
					       <el-button @click="unSubmit">取消</el-button>
					     </el-form-item>
					   </el-form>
					   </div>
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
			  visible:false,
			  form:{
				    dd_id:null,
				    id:null,
			        paymentmethod:'',
					dgname:'', 
					number:null,
					munit:'',
					status:'',
					rname:'',
					creat_time:''
			            
			     },
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
				   url: "http://localhost:8188/service/DrugDedposit",
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
		      updatehandleClick(row) {
				this.visible=true;             
				this.form=row;
				console.log(this.form);
		      },
			  onSubmit(){
				  let DrugRegResult  =this.form;
				  console.log(DrugRegResult);
				  this.axios({
					   url: 'http://localhost:8188/service/DrugDedpositupdate',
					   method: 'POST',
					    data:DrugRegResult							 			
					   }).then(function(response) {
					   			  			 				
					   console.log(response)
					   	  								 		
					   }).catch(function(err) {
					    console.log("上传失败")
	
					    console.log(err)
					   	});
				  
				  this.visible=false;

				  
				  
				  
			  },
			  unSubmit(){
				  this.visible=false;
				  
			  },
			  
			  		deleteRow(index, rows,row) {
			  								        var id=row.dd_Id;
			  								         this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
			  								           confirmButtonText: '确定',
			  								           cancelButtonText: '取消',
			  								           type: 'warning'
			  								         }).then(() => {
			  									
			  											 rows.splice(index, 1);
			  										     this.axios({
			  											 url: 'http://localhost:8188/service/DrugDedpositdelectbyid/'+id,
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
	.visible{
		padding: 30px;
		position: absolute;
		background-color:ghostwhite ;
		border:2px #F0FFFF solid;
		left: 45%;
		top: 10%;
	}
</style>
