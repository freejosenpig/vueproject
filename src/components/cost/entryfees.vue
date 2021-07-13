<template>
	<h1>缴费界面管理</h1>
	<div>
	  <el-form :inline="true" :model="formInline" class="demo-form-inline">
	  	  <el-form-item label="老人姓名">
	  		<el-input v-model="formInline.user" placeholder="老人姓名"></el-input>
	  	  </el-form-item>
	  	  <el-form-item>
	  		<el-button type="primary" @click="onSubmit">查询</el-button>
	  	  </el-form-item>
	  </el-form>
	</div>
	<!-- 入住缴费 -->
	<div style="width:100%;height: 50px;" class="addiv" v-bind:style="styleObject">
		<el-col>
			<el-form :model="jiaofeiForm">
				缴费老人:
				<el-input v-model="jiaofeiForm.oldpeopleName" style="width: 130px;"></el-input>
				应缴总额:
				<el-input v-model="jiaofeiForm.entryfeesMoney" style="width: 130px;"></el-input>
				实收金额:
				<el-input v-model="jiaofeiForm.shiMoney" style="width: 130px;"></el-input>
				<el-button @click="addEntry" style="width: 130px;margin-left: 20px;" type="primary">确定缴费</el-button>
				<el-button @click="addcancel" style="width: 130px;margin-left: 20px;" type="danger">取消缴费</el-button>
			</el-form>
		</el-col>
	</div>
	<el-table  :data="tableData" style="width: 100%" :row-class-name="tableRowClassName">
	  <el-table-column type="index"  label="编号" align="center">  </el-table-column>
	  <el-table-column  prop="jdOldpeople.oldpeopleName" label="老人姓名" align="center" >  </el-table-column>
	  <!-- <el-table-column prop="jdOldpeople.oldpeopleAccount" label="床位号" align="center" ></el-table-column> -->
	  <el-table-column prop="entryfeesMoney" label="缴费金额" align="center"> </el-table-column>
	  <el-table-column prop="jdOldpeople.oldpeopleName" label="缴费人" align="center"></el-table-column>
	  <el-table-column prop="entryfeesOpid" label="收银员" align="center">admin</el-table-column>
	  <el-table-column prop="entryfeesTime" label="缴费日期" sortable align="center"> </el-table-column>
	  <el-table-column prop="entryfeesExplain" label="收费说明" align="center"> </el-table-column>
	  <el-table-column prop="entryfeesstatus" label="缴费状态" align="center"> 
		<template #default="scope">
			<p v-if="scope.row.entryfeesstatus==0">未缴费</p>
			<p v-if="scope.row.entryfeesstatus==1">已缴费</p>
		</template>
	   </el-table-column>
	  <el-table-column  label="操作" align="center">
		  <template #default="scope">
		       <p v-if="scope.row.entryfeesstatus==0">
				    <el-button  size="mini" type="danger"  @click="handleJiaofei(scope.row)">缴费</el-button>
			   </p>
			   <p v-if="scope.row.entryfeesstatus==1">
					<el-button size="mini" type="primary" circle="" icon="el-icon-check"></el-button>
			   </p>
		  </template>
	  </el-table-column>
	</el-table>
	
	
</template>

<script>
	import { ElMessage } from 'element-plus'
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
				jiaofeiForm: {
					oldpeopleName:"",
					entryfeesMoney: "",
					shiMoney: "",
					lastmoney: "",
					oldpeopleId:""
				},
				updateAccount1:{
					oldpeopleId:"",oldpeopleAccount:""
				},
				//老人账户余额
				totalAccount:"",
				styleObject: {
					display: 'none'
				},
				listForm:{
					oldpeopleId:"",
					listMoeny:"",
					listExplain:"",
					listRemark:""
				},
				qianfeiForm:{
					oldpeopleId:"",
					outstandingMoney:"",
					outstandingremark:""
				},
				
			}
		},
		methods:{
			//新增欠费记录
			insertQianfei(id,shi,entry,account){
				const _this=this
				this.qianfeiForm.oldpeopleId=id
				console.log(this.qianfeiForm.oldpeopleId+"ddd")
				this.qianfeiForm.outstandingMoney=entry-shi+account
				console.log(this.qianfeiForm.outstandingMoney+"fff")
				this.qianfeiForm.outstandingremark="入住缴费欠费"
				this.axios.post("http://localhost:8188/insertOutstanding",this.qianfeiForm)
				.then(function(response){
					console.log(response)
				}).catch(function(error){
					console.log(error)
				})
			},
			//新增一条流水
			insertLCostist(oldpeopleId,lastmoney){
				const _this=this
				this.listForm.oldpeopleId=oldpeopleId
				this.listForm.listMoeny=lastmoney
				this.listForm.listExplain="入住缴费"
				this.listForm.listRemark="无"
				this.axios.post("http://localhost:8188/insertLCostist",this.listForm)
				.then(function(response){
					console.log(response)
				}).catch(function(error){
					console.log(error)
				})
			},
			//确认缴费(修改缴费状态为已缴费)
			addEntry(){
				const _this=this
				this.axios.put("http://localhost:8188/updateEntryByKey",this.jiaofeiForm)
				.then(function(response){
					console.log(response)
					console.log(_this.jiaofeiForm.oldpeopleId+"kkk"+_this.jiaofeiForm.entryfeesMoney)
					_this.updateAccount(_this.jiaofeiForm.oldpeopleId,_this.jiaofeiForm.entryfeesMoney)
					//查询老人账户余额
					_this.axios.get("http://localhost:8188/selectByPrimaryKey/"+_this.jiaofeiForm.oldpeopleId)
					.then(function(response){
						console.log(response)
						_this.totalAccount=response.data.data.oldpeopleAccount
						console.log(_this.totalAccount+"bbbb")
						if(_this.totalAccount<_this.jiaofeiForm.shiMoney){
							//新增欠费记录
							_this.insertQianfei(_this.jiaofeiForm.oldpeopleId,_this.jiaofeiForm.shiMoney,_this.jiaofeiForm.entryfeesMoney,_this.totalAccount)
							_this.insertLCostist(_this.jiaofeiForm.oldpeopleId,_this.jiaofeiForm.shiMoney)
						}else{
							//新增流水记录
							_this.insertLCostist(_this.jiaofeiForm.oldpeopleId,_this.jiaofeiForm.shiMoney)
						}
					}).catch(function(error){
						console.log(error)
					})
					ElMessage.success({
					   message: '入住缴费成功',
					   type: 'success'
					})
					setTimeout(function () {
					  window.location.reload()
					}, 1000)
				}).catch(function(error){
					console.log(error)
				})
			},
			//根据老人编号修改账户余额
			updateAccount(OldId,account){
				const _this=this
				this.updateAccount1.oldpeopleId=OldId
				this.updateAccount1.oldpeopleAccount=account
				console.log(this.updateAccount1.oldpeopleId+"bbbb")
				console.log(this.updateAccount1.oldpeopleAccount+"cccc")
				//根据老人编号查询所有老人信息
				this.axios.get("http://localhost:8188/selectByPrimaryKey/"+this.updateAccount1.oldpeopleId)
				.then(function(response){
					console.log(response)
					_this.totalAccount=response.data.data.oldpeopleAccount
					console.log(_this.totalAccount+"总账户")
					console.log(response.data.data.oldpeopleAccount+"总账户")
					_this.updateAccount1.oldpeopleAccount=_this.totalAccount-_this.jiaofeiForm.shiMoney
					console.log(_this.updateAccount1.oldpeopleAccount+"剩余金额")
					_this.axios.put("http://localhost:8188/updateAccount",_this.updateAccount1)
					.then(function(response){
						console.log(response)
					}).catch(function(error){
						console.log(error)
					})
				}).catch(function(error){
					console.log(error)
				})
				
			},
			//显示缴费
			handleJiaofei(row) {
				const _this = this
				this.jiesuanDia = true
				this.styleObject.display = 'block'
				this.jiaofeiForm.oldpeopleName = row.jdOldpeople.oldpeopleName
				this.jiaofeiForm.oldpeopleId = row.jdOldpeople.oldpeopleId
				this.jiaofeiForm.entryfeesMoney = row.entryfeesMoney
				console.log( row.entryfeesMoney+"hhhh")
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
