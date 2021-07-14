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
			           width="200">
			           <template #default="scope">
						  <el-button @click="addrow(scope.$index, tableData,scope.row)" type="text" size="small">添加</el-button>
			              <el-button @click="updatehandleClick(scope.row)" type="text" size="small">修改</el-button>
			              <el-button type="text" size="small" @click="deleteRow(scope.$index, tableData,scope.row)" >销户</el-button>
			           </template>
			         </el-table-column>
			       </el-table>
				   <div v-show="visible" class="visible">
				   <el-form ref="form" :model="form" label-width="80px">
				    
				   						  <el-form-item label="体检医院">
				   						    <el-input v-model="form.hospital"></el-input>
				   						  </el-form-item>
				   						  <el-form-item label="体检时间">
				   						    <el-input v-model="form.etime"></el-input>
				   						  </el-form-item>
				   						  <el-form-item label="体检结果">
				   						    <el-input v-model="form.eresult"></el-input>
				   						  </el-form-item>
				   						  <el-form-item label="登记人id">
				   						    <el-input v-model="form.registrant"></el-input>
				   						  </el-form-item>
				   							<el-form-item>
				   								<el-date-picker
				   										        v-model="form.rgtime"
				   										        type="datetime"
				   										        placeholder="登记时间">
				   										      </el-date-picker>
				   							</el-form-item>
											<el-form-item>
											<el-date-picker
											      v-model="form.create_time"
											      type="datetime"
											      placeholder="创建时间">
											    </el-date-picker>
											</el-form-item>			  
				   										  
				        <el-form-item>
				         <el-button type="primary" @click="onSubmit">立即修改</el-button>
				         <el-button @click="unSubmit">取消</el-button>
				       </el-form-item>
				     </el-form>
				     </div>
					 <div v-show="visible2" class="visible">
					 <el-form ref="form" :model="form1" label-width="80px">
					                            <el-form-item label="老人id">
					                              <el-input v-model="form1.id"></el-input>
					                            </el-form-item>
					 						  <el-form-item label="体检医院">
					 						    <el-input v-model="form1.hospital"></el-input>
					 						  </el-form-item>
					 						  <el-form-item label="体检时间">
					 						    <el-input v-model="form1.etime"></el-input>
					 						  </el-form-item>
					 						  <el-form-item label="体检结果">
					 						    <el-input v-model="form1.eresult"></el-input>
					 						  </el-form-item>
					 						  <el-form-item label="登记人id">
					 						    <el-input v-model="form1.registrant"></el-input>
					 						  </el-form-item>
					 							<el-form-item>
					 								<el-date-picker
					 										        v-model="form1.rgtime"
					 										        type="datetime"
					 										        placeholder="登记时间">
					 										      </el-date-picker>
					 							</el-form-item>
					 											<el-form-item>
					 											<el-date-picker
					 											      v-model="form.create_time"
					 											      type="datetime"
					 											      placeholder="创建时间">
					 											    </el-date-picker>
					 											</el-form-item>			  
					 										  
					      <el-form-item>
					       <el-button type="primary" @click="addSubmit">立即添加</el-button>
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
			  visible2:false,
			  form:{
			  				  er_Id:null,
							  id:null,
							  hospital:'',
							  etime:'2018-01-13T06:15:27.000+00:00',
							  eresult:'',
							  registrant:null,
							  rgtime:'',
							  create_time:''
			  				 
			  				 
			            
			     },
				 form1:{
				 				             
				 							  id:null,
				 							  hospital:'',
				 							  etime:'',
				 							  eresult:'',
				 							  registrant:null,
				 							  rgtime:'',
				 							  create_time:''
				 				 
				 				 
				           
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
			   updatehandleClick(row) {
			   				this.visible=true;             
			   				this.form=row;
			   				console.log(this.form);
			   },
			   onSubmit(){
			   				  let ExaminationRegister =this.form;
			   				  console.log(ExaminationRegister);
			   				  this.axios({
			   					   url: 'http://localhost:8188/service/ExaminationRegistUpdate',
			   					   method: 'POST',
			   					    data:ExaminationRegister							 			
			   					   }).then(function(response) {
			   					   			  			 				
			   					   console.log(response)
			   					   	  								 		
			   					   }).catch(function(err) {
			   					    console.log("上传失败")
			   	
			   					    console.log(err)
			   					   	});
			   				  
			   				  this.visible=false;
			   
			   				  
			   				  
			   				  
			   },
			   unSubmit(){
				              this.form={},
			   				  this.visible=false;
							  this.visible2=false;
			   				  
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
			  	  		 			 			 
			  	  		 			 											 	   	
			  	  		 			 											 
			  	  		 			 			
			  	  		 			       },
									addrow(index,rows,row){
										this.visible2=true;
										this.form1 =row;
										rows.splice(index+1,row,this.form1
										)
																	   
									},
									addSubmit(){
										let ExaminationRegister =this.form1;
										this.axios({
										 url: 'http://localhost:8188/service/ExaminationRegistInsert/',
										 method: 'post',
										 data:ExaminationRegister
										  						 			
										 }).then(function(response) {
										 			  			 	 			  			 													  
										console.log("提交")
										 console.log(response)
										 	  								 		
										 }).catch(function(err) {
										  
										 console.log(id)	
										  console.log(err)
										 	})
										this.visible2=false;
										
										
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
