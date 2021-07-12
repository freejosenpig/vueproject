<template>
	<h1>退住界面管理</h1>
	<div>
	  <el-form :inline="true" :model="formInline" >
	  	  <el-form-item label="老人姓名">
	  		<el-input v-model="formInline.oldpeopleName" placeholder="老人姓名" clearable></el-input>
	  	  </el-form-item>
	  	  <!-- <el-form-item label="退住金额">
	  		<el-input v-model="formInline.value1" style="width: 100px;"/>-<el-input v-model="formInline.value2"  style="width: 100px;"/>
	  	  </el-form-item> -->
	  	  <el-form-item>
	  		<el-button type="primary" @click="selectBynam">查询</el-button>
	  	  </el-form-item>
	  </el-form>
	</div>
	
	 <el-table  :data="tableData" style="width: 100%" >
	  <el-table-column type="index"  label="编号" align="center">  </el-table-column>
	  <el-table-column  prop="jdOldpeople.oldpeopleName" label="老人姓名" align="center"></el-table-column>
	  <el-table-column prop="jdOldpeople.oldpeopleAddtime" label="入院时间" align="center"></el-table-column>
	  <el-table-column prop="prestoreMoney" label="结算时间" align="center"> </el-table-column>
	 <el-table-column prop="istoback" label="结算状态" align="center">
		  <template #default="scope">
			  <p v-if="scope.row.istoback==0">未结算</p>
			  <p v-if="scope.row.istoback==1">已结算</p>
		  </template>
	  </el-table-column>
	  <el-table-column label="操作" align="center">
		  <template #default="scope">
		       <el-button size="mini" @click="handleEdit(scope.$index,scope.row)">编辑</el-button>
		        <el-button size="mini" type="primary"  @click="handleJieSuan(scope.row)">结算</el-button>
		  </template>
	  </el-table-column>
	</el-table>
	<!-- 退住结算 -->
	<div style="width:100%;height: 50px;" class="addiv"  v-bind:style="styleObject">
		<el-col>
			 缴费老人:
			 <el-input prop="jiesuanForm.oldpeopleName" style="width: 130px;"></el-input>
			 应缴总额:
			 <el-input prop="jiesuanForm.backEntryfeesMoney" style="width: 130px;"></el-input>
			 应退总额:
			 <el-input prop="jiesuanForm.backMoney" style="width: 130px;"></el-input>
			 <p v-if="jiesuanForm.lastmoney<0">
				实收金额:
				<el-input prop="prestoreMoney" style="width: 130px;"></el-input>
			 </p>
			 <p v-if="jiesuanForm.lastmoney<0">
			 	应退金额:
			 	<el-input prop="prestoreMoney" style="width: 130px;"></el-input>			 
			 </p>
			 <el-button @click="addPrestore" style="width: 130px;margin-left: 20px;" type="primary">确定结算</el-button>
			 <el-button @click="addcancel" style="width: 130px;margin-left: 20px;" type="danger">取消结算</el-button>
		</el-col>
	</div>
</template>

<script>
	export default{
		data(){
			return{
				formInline:{
					oldpeopleName:""
				},
				tableData:[],
				jiesuanDia:false,
				jiesuanForm:{
					oldpeopleName:"",backEntryfeesMoney:"",backMoney:"",
					lastmoney:""
				},
				styleObject: {
				    display: 'none'
				}
			}
		},
		methods:{
			handleJieSuan(row){
				const _this=this
				this.jiesuanDia=true
				this.styleObject.display='block'
				console.log(row.oldpeopleId)
				this.axios.get("http://localhost:8188/selectBackByoldId/"+row.oldpeopleId)
				.then(function(response){
					console.log(response)
					_this.jiesuanForm=response.data
					_this.jiesuanForm.backEntryfeesMoney=response.data.backEntryfeesMoney
					_this.jiesuanForm.lastmoney=response.data.backMoney-response.data.backEntryfeesMoney
				}).catch(function(error){
					console.log(error)
				})
			},
			selectBynam(){
				const _this=this
				this.axios.get("http://localhost:8188/selectBycontion/"+this.formInline.oldpeopleName)				.then(function(response){
					console.log(response)
					_this.tableData=response.data
				}).catch(function(error){
					console.log(error)
					
				})
							
			},
			addcancel(){
				this.styleObject.display='none'
			}
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
