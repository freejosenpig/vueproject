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
			     prop="medical_Time"
			     label="就医时间"
			     width="120">
			   </el-table-column>
			   <el-table-column
			     prop="medical_Adress"
			     label="就医地址"
			     width="120">
			   </el-table-column>
			   <el-table-column
			     prop="doctor"
			     label="医生"
			      width="120">
			    </el-table-column>
			         <el-table-column
			           prop="diagnosis_Results"
			           label="诊断结果"
			           width="500">
			         </el-table-column>
			         <el-table-column
			           prop="medical_Advice"
			           label="医嘱"
			           width="120">
			         </el-table-column>
			   	  <el-table-column
			   	    prop="rgtime"
			   	    label="登记时间"
			   	    width="120">
			   	  </el-table-column>
			   	  <el-table-column
			   	    prop="creat_time"
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
			    
			   						  <el-form-item>
			   						  	<el-date-picker
			   						  			        v-model="form.medical_Time"
			   						  			        type="datetime"
			   						  			        placeholder="入住时间">
			   						  			      </el-date-picker>
			   						  </el-form-item>
			   						  <el-form-item label="医院地址">
			   						    <el-input v-model="form.medical_Adress"></el-input>
			   						  </el-form-item>
			   						  <el-form-item label="医生">
			   						    <el-input v-model="form.doctor"></el-input>
			   						  </el-form-item>
			   						  <el-form-item label="诊断结果">
			   						    <el-input v-model="form.diagnosis_Results"></el-input>
			   						  </el-form-item>
									  <el-form-item label="建议">
									    <el-input v-model="form.medical_Advice"></el-input>
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
				 <el-form ref="form" :model="form1" label-width="200px">
					 <el-form-item label="老人id">
					   <el-input v-model="form1.id"></el-input>
					 </el-form-item>
					 <el-form-item>
					   	<el-date-picker
					   			        v-model="form1.medical_Time"
					   			        type="datetime"
					   			        placeholder="入住时间">
					   			      </el-date-picker>
					   </el-form-item>
					   <el-form-item label="医院地址">
					     <el-input v-model="form1.medical_Adress"></el-input>
					   </el-form-item>
					   <el-form-item label="医生">
					     <el-input v-model="form1.doctor"></el-input>
					   </el-form-item>
					   <el-form-item label="诊断结果">
					     <el-input v-model="form1.diagnosis_Results"></el-input>
					   </el-form-item>
					   <el-form-item label="建议">
					     <el-input v-model="form1.medical_Advice"></el-input>
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
			  		mr_Id:null,
					id:null,
					medical_Time:"",
					medical_Adress:"",
					doctor:"",
					diagnosis_Results:"",
					medical_Advice:"",
					rgtime:"",
				    creat_Time:""
			  				 
			            
			     },
				 form1:{
	
				 					id:null,
				 					medical_Time:"",
				 					medical_Adress:"",
				 					doctor:"",
				 					diagnosis_Results:"",
				 					medical_Advice:"",
				 					rgtime:"",
				 				    creat_Time:""
				 				 
				           
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
				   url: "http://localhost:8188/service/medicalRegisterImp",
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
			  				  let MedicalRegistration =this.form;
			  				  console.log();
			  				  this.axios({
			  					   url: 'http://localhost:8188/service/medicalRegisterUpdate',
			  					   method: 'POST',
			  					   data:MedicalRegistration							 			
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
			    		 						        var id=row.mr_Id;
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
			    		 			 			 
			    		 			 											 	   	
			    		 			 											 
			    		 			 			
			    		 			       },
										   addrow(index,rows,row){
										   	this.visible2=true;
										   	this.form1 =row;
										   	rows.splice(index+1,row,this.form1
										   	)
										   								   
										   },
										   addSubmit(){
										   	let MedicalRegistration =this.form1;
										   	this.axios({
										   	 url: 'http://localhost:8188/service/MedicalRegistrationInsert/',
										   	 method: 'post',
										   	 data:MedicalRegistration
										   	  						 			
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
