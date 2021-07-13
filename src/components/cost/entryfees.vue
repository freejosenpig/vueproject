<template>
	<h1>缴费界面管理</h1>
	<div>
	  <el-form :inline="true" :model="formInline" class="demo-form-inline">
	  	  <el-form-item label="老人姓名">
	  		<el-input v-model="formInline.user" placeholder="老人姓名"></el-input>
	  	  </el-form-item>
	  	  <el-form-item label="缴费金额">
	  		<el-input v-model="formInline.user" style="width: 100px;"/>-<el-input v-model="formInline.user"  style="width: 100px;"/>
	  	  </el-form-item>
	  	  <el-form-item>
	  		<el-button type="primary" @click="onSubmit">查询</el-button>
	  	  </el-form-item>
	  </el-form>
	</div>
	<!-- 入住缴费 -->
	<div style="width:100%;height: 50px;" class="addiv" v-bind:style="styleObject">
		<el-col>
			<el-form :model="jiesuanForm">
				缴费老人:
				<el-input v-model="jiesuanForm.oldpeopleName" style="width: 130px;"></el-input>
				应缴总额:
				<el-input v-model="jiesuanForm.backEntryfeesMoney" style="width: 130px;"></el-input>
				实收金额:
				<el-input v-model="jiesuanForm.backMoney" style="width: 130px;"></el-input>
				<el-button @click="addPrestore" style="width: 130px;margin-left: 20px;" type="primary">确定结算</el-button>
				<el-button @click="addcancel" style="width: 130px;margin-left: 20px;" type="danger">取消结算</el-button>
			</el-form>
		</el-col>
	</div>
	<el-table  :data="tableData" style="width: 100%" :row-class-name="tableRowClassName">
	  <el-table-column type="index"  label="编号" align="center">  </el-table-column>
	  <el-table-column  prop="jdOldpeople.oldpeopleName" label="老人姓名" align="center" >  </el-table-column>
	  <el-table-column prop="jdOldpeople.oldpeopleAccount" label="床位号" align="center" ></el-table-column>
	  <el-table-column prop="entryfeesMoney" label="缴费金额" align="center"> </el-table-column>
	  <el-table-column prop="jdOldpeople.oldpeopleName" label="缴费人" align="center"> </el-table-column>
	  <el-table-column prop="entryfeesOpid" label="收银员" align="center"> </el-table-column>
	  <el-table-column prop="entryfeesTime" label="缴费日期" sortable align="center"> </el-table-column>
	  <el-table-column prop="entryfeesExplain" label="收费说明" align="center"> </el-table-column>
	  <el-table-column  label="操作" align="center">
		  <template #default="scope">
		        <el-button  size="mini" type="danger"  @click="handleJiaofei(scope.row)">缴费</el-button>
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
				//老人信息
				olddata:[],
				tableData:[],
				jiesuanForm: {
					oldpeopleName: "",
					backEntryfeesMoney: "",
					backMoney: "",
					lastmoney: "",
					oldpeopleId:""
				},
				styleObject: {
					display: 'none'
				}
			}
		},
		methods:{
			handleJiaofei(row) {
				const _this = this
				this.jiesuanDia = true
				this.styleObject.display = 'block'
				this.axios.get("http://localhost:8188/selectEntryAll")
				.then(function(response){
					console.log(response)
				}).catch(function(error){
					console.log(error)
				})
			},
			addcancel() {
				this.styleObject.display = 'none'
			}
		},
		created() {
			const _this=this
			this.axios.get("http://localhost:8188/selectEntryAll")
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
